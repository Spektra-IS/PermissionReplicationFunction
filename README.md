# PermissionReplicationFunction

## Um lausnina
Þegar búið er að fara í gegnum skrefin hér að neðan verður til Azure Function App í tenant hjá ykkur. Þetta Function App mun vera með réttindi til að bæta við öppum frá Spektra sem þurfa að hafa réttindi inn á öll WorkPoint Site Collections, eins og `Spektra eSignature - Limited`.

Dæmi:
Þið erum með rafrænar undirritanir í WorkPoint en viljið ekki gefa appi sem spektra notar til að sækja skjöl úr WorkPoint réttindin `Sites.FullControl.All`, sem myndi gefa því réttindi á öll Site Collection í tenant. Þetta Function App er þá sett upp og það sér um að veita viðeigandi réttindi bara inn á þau Site Collections sem eru undir WorkPoint (skilgreint í `Allowed Url Paths` þegar function er sett upp).

## 📦 Skrár

- `template.json`: ARM template defining the Function App, storage account, App Insights, and managed identity.
- `parameters.json`: Parameter values for deployment.

## 🚀 Setja upp í Azure

Smelltu á takkan hér að neðan og fylgdu [leiðbeiningum](instructions.md) til að setja þetta Function App upp í Azure í gegnum Azure Portal:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FSpektra-IS%2FPermissionReplicationFunction%2Fmain%2Ftemplate.json)

<!-- > You can modify parameter values during deployment or use a custom `parameters.json` file. -->
<!-- > Þú getur breytt gildum í `parameters.json` í ferlinu -->
