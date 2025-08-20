# Um lausnina
Þegar búið er að fara í gegnum skrefin hér að neðan verður til Azure Function App í tenant hjá ykkur. Þetta Function App mun vera með réttindi til að bæta við öppum frá Spektra sem þurfa að hafa réttindi inn á öll WorkPoint Site Collections, eins og `Spektra eSignature - Limited`. 

Dæmi:
Þið erum með rafrænar undirritanir í WorkPoint en viljið ekki gefa appi sem spektra notar til að sækja skjöl úr WorkPoint réttindin `Sites.FullControl.All`, sem myndi gefa því réttindi á öll Site Collection í tenant. Þetta Function App er þá sett upp og það sér um að veita viðeigandi réttindi bara inn á þau Site Collections sem eru undir WorkPoint (skilgreint í `Allowed Url Paths` þegar function er sett upp).



# Hvað þarf að gera
### Setja inn Function App

1. Fara inn á [Spektra-PermissionReplicationFunction](https://github.com/Spektra-IS/PermissionReplicationFunction) á github.
2. Smella á `Deploy to Azure`
3. Afrita það sem er í `parameters.json` á github, fara svo í "Edit parameters" flipann í glugganum sem opnaðist þegar smellt far á "Deploy to Azure" og líma þangað inn.
4. Breyta svo eftirfarandi gildum áður en smellt er á Save
    * `appInfoJson` þar má bæta við fleiri öppum sem eiga að fara inn á það Site Collection sem er verið að synca réttindi á.
    * `spHostUrl` skipta út 'yourtenant' fyrir ykkar tenant.
    * `allowedUrlPaths` skilgreina þarna rótina á WorkPoint. Ef það á að nota þetta function app fyrir margar WorkPoint lausnir í ykkar tenant þá þarf að skilgreina þær þarna, aðskilið með kommu ",".
        1. Dæmi fyrir eina wp rót
        ```json
        "allowedUrlPaths": {
            "value": "/sites/workpoint"
        }
        ```
        2. Dæmi fyrir 2 wp rætur
        ```json
        "allowedUrlPaths": {
            "value": "/sites/workpoint_ABC_0,/sites/workpoint_DEF_0"
        }
        ```
5. Smella svo á Save
6. Velja svo rétt subscription og Resource group (Búa til nýja Resource Group ef þarf) og region.
7. Gefa nafn á function appið
8. :exclamation: Ekki breyta Location það á að vera `[resourceGroup().location]`
9. `Deploy Storage Account`
    * Ef það er `true` þá mun Azure stofna nýjan storage account.
    * Ef það er `false` þá þarf að skilgreina nafnið á storage account sem function appið á að nota í `Existing Storage Account Name`.
10. `Deploy App Insights`
    * Ef það er `true` þá mun Azure stofna nýtt Application Insights.
    * Ef það er `false` þá þarf að skilgreina "Instrumentation Key" á því Application Insights sem á að nota í `Existing App Insights InstrumentationKey`
11. `Package URI` Þarna fer slóðin sem þið fáið frá Spektra fyrir zip skrána sem function appið mun nota.
12. `App Info Json` Þetta ætti að vera komið inn rétt frá `parameters.json`
13. `Sp Host Url` Þetta ætti að vera komið inn rétt frá `parameters.json`
14. `Allowed Url Paths` Þetta ætti að vera komið inn rétt frá `parameters.json`
15. Smella svo á "Review + create"

### Gefa Function app-i réttindi til að bæta við réttindum í SharePoint
Þegar Function app-ið er komið inn þá þarf að gefa því Graph API `Sites.FullControl.All`
1. Finna object (principal) ID á function appið sem var verið að stofna
    * Það er hægt að finna það með því að fara inn á function appið sem var verið að stofna og smella á "Identity" sem er undir "Settings"
2. Í powershell, tengjast SharePoint með PnP.PowerShell með notanda sem er admin í Azure
3. Keyra eftirfarandi skipun
```PowerShell
Add-PnPAzureADServicePrincipalAppRole -Principal "{id frá skrefi 1}" -AppRole "Sites.FullControl.All" -BuiltInType MicrosoftGraph
```

### Senda Spektra slóð á Function
Þegar réttindin eru komin á function appið þá þarf að senda okkur slóðina sem við getum notað til að kalla í þetta function hjá ykkur.
1. Fara inn á function appið og í overview smella á function sem heitir `AddAppsToSites`
2. Smella á "Get function URL" og afrita slóðina sem er í reitnum `default (Function key)`


# Hvernig þetta virkar
### Dæmi með rafrænar undirritanir
Þegar WorkPoint stofnar nýtt svæði fyrir færslu í WorkPoint þá sendist event á Azure Event Bus hjá Spektra. Þegar við fáum event til okkar þá sækjum við slóðina á Site Collection-ið sem svæðið stofnaðist á. Ef appið sem við notum til að sækja skrár til að senda í undirritun er ekki með aðgang að því Site Collection-i sem svæðið stofnaðist á þá sendum við `POST` á slóðina á ykkar Function Appi og það sér um að bæta réttindum á Site Collection-ið.


:mega: ***ATH: Function Appið mun bara bæta við réttindum á þau svæði innihalda URL úr því sem er skilgreint í `Allowed URL Paths` og því ekki hægt að nota þetta til að bæta réttindum við Site Collections þar sem slóð byrjar ekki á þeim gildum sem eru skilgreind þar.***
