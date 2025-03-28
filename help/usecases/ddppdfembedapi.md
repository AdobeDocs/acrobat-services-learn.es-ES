---
title: Publicación de documentos digitales
description: Aprenda a mostrar documentos de PDF incrustados en páginas web mediante la API Adobe PDF Embed
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8090
thumbnail: KT-8090.jpg
exl-id: 3aa9aa40-a23c-409c-bc0b-31645fa01b40
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1722'
ht-degree: 0%

---

# Publicación de documentos digitales

![Banner de héroe de caso de uso](assets/UseCaseDigitalHero.jpg)

Los documentos electrónicos están en todas partes; de hecho, probablemente haya [billones de PDF](https://itextpdf.com/en/blog/technical-notes/do-you-know-how-many-pdf-documents-exist-world) en todo el mundo y esa cifra aumenta todos los días. Al incrustar un visor de PDF en las páginas web, permite a los usuarios ver documentos sin tener que rediseñar el HTML y el CSS, ni obstruir el acceso al sitio web.

Vamos a explorar un escenario popular. Una empresa publica [documentos técnicos en su sitio web](https://developer.adobe.com/document-services/use-cases/content-publishing/digital-content-publishing)
para proporcionar contexto a sus aplicaciones y servicios. El responsable de marketing del sitio web quiere entender mejor cómo los usuarios interactúan con su contenido basado en PDF e incorporarlo a su página web y marca. Han decidido publicar los informes técnicos como [contenido cerrado](https://whatis.techtarget.com/definition/gated-content-ungated-content#:~:text=Gated%20content%20is%20online%20materials,about%20their%20jobs%20and%20organizations.), para controlar quién puede descargarlos.

## Lo que puedes aprender

En este tutorial práctico, aprende a mostrar documentos de PDF incrustados en páginas web con la [API Adobe PDF Embed](https://developer.adobe.com/document-services/apis/pdf-embed), que es gratuita y fácil de usar. En estos ejemplos se utilizan JavaScript, Node.js, Express.js, HTML y CSS. Puede ver el código completo del proyecto en [GitHub](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1).

## API y recursos relevantes

* [API de incrustación de PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API de servicios de PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Código del proyecto](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1)

## Crear una aplicación Web de nodos

Empecemos creando un sitio usando Node.js y Express que usa una plantilla bonita y ofrece varios PDF para descargar.

Primero, [descargue e instale Node.js](https://nodejs.org/en/download/).

Para crear un proyecto Node.js fácilmente con una estructura de aplicación web mínima, instale la herramienta de generación de aplicaciones `` `express-generator` ``.

```
npm install express-generator -g
```

A continuación, cree la nueva aplicación Express denominada pdf-app, eligiéndola como motor de visualización.

```
express pdf-app --view=ejs
```

A continuación, vaya al directorio \\pdf-app e instale todas las dependencias del proyecto.

```
cd pdf-app
npm install
```

A continuación, inicie el servidor web local y ejecute la aplicación.

```
npm start
```

Por último, abra el sitio web en <http://localhost:3000>.

![Captura de pantalla del sitio web básico](assets/ddp_1.png)

Ahora dispone de un sitio web básico.

## Representación de datos del informe técnico

Para publicar artículos técnicos en el sitio web, los datos del informe técnico se definen y preparan en el sitio web para mostrar estos documentos. En primer lugar, cree una nueva carpeta \\data en la raíz del proyecto. La información de los documentos técnicos disponibles procede de un nuevo archivo denominado [data.json](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/data/data.json), que se coloca en la carpeta de datos.

Para darle a la aplicación web un aspecto elegante y refinado, instala las bibliotecas de aplicaciones de [Bootstrap](https://getbootstrap.com/) y [Font Awesome](https://fontawesome.com/).

```
npm install bootstrap
npm install font-awesome
```

Abra el archivo app.js e incluya estos directorios como orígenes de archivos estáticos, colocándolos después de la línea `` `express.static` `` existente.

```
app.use(express.static(path.join(__dirname, '/node_modules/bootstrap/dist')));
app.use(express.static(path.join(__dirname, '/node_modules/font-awesome')));
```

Para incluir los documentos de PDF, cree una carpeta denominada \\pdf bajo la carpeta \\public del proyecto. En lugar de crear los PDF y las miniaturas por sí mismo, puede copiarlos de esta carpeta del repositorio [GitHub](https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app/public) a las carpetas \\pdfs y \\image.

La carpeta \\public\\pdfs contiene ahora los documentos del PDF:

![Captura de pantalla de los iconos de archivo del PDF](assets/ddp_2.png)

Mientras que la carpeta \\public\\images debe contener las miniaturas de cada uno de los documentos del PDF:

![Captura de pantalla de miniaturas de PDF](assets/ddp_3.png)

A continuación, abra el archivo \\paths\\index.js, que contiene la lógica para enrutar la página principal. Para utilizar los datos del informe técnico del archivo data.json, debe cargar el módulo Node.js responsable de acceder al sistema de archivos e interactuar con él. A continuación, declare la constante `fs` en la primera línea del archivo \\paths\\index.js, de la siguiente manera:

```
const fs = require('fs');
```

A continuación, lea y analice el archivo data.json y almacénelo en la variable papers :

```
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
```

Ahora modifique la línea para invocar el método render de la vista de índice, pasando la colección papers como modelo de la vista de índice.

```
res.render('index', { title: 'Embedding PDF', papers: papers });
```

Para representar la colección de documentos técnicos en la página principal, abra el archivo \\views\\index.ejs y reemplace el código existente por el código del [archivo de índice](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/views/index.ejs) del proyecto.

A continuación, vuelva a ejecutar npm start y abra <http://localhost:3000> para ver su colección de documentos técnicos disponibles.

![Captura de pantalla de miniaturas de artículos técnicos](assets/ddp_4.png)

En las siguientes secciones, mejore el sitio web y utilice [PDF Embed API](https://developer.adobe.com/document-services/apis/pdf-embed) para mostrar los documentos de PDF en la página web. La API PDF Embed es de uso gratuito; solo tiene que obtener una credencial de API.

## Obtención de una credencial de API de incrustación de PDF

Para obtener una credencial de API incrustada de PDF gratuita, visita la página [Introducción](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) después de registrarte para obtener una nueva cuenta o iniciar sesión en tu cuenta existente.

Haga clic en **Crear nuevas credenciales** y, a continuación, en **Introducción:**

![Captura de pantalla de cómo crear nuevas credenciales](assets/ddp_5.png)

En este punto, se le pedirá que se registre para obtener una cuenta gratuita si no tiene una.

Seleccione **PDF Embed API** y, a continuación, escriba el nombre de las credenciales y el dominio de la aplicación. Use el dominio **localhost** porque se ha probado la aplicación web localmente.

![Captura de pantalla de la creación de nuevas credenciales para la API de incrustación de PDF](assets/ddp_6.png)

Haga clic en el botón **Crear credenciales** para acceder a sus credenciales de PDF y obtener el ID de cliente (clave de API).

![Captura de pantalla de cómo copiar nuevas credenciales](assets/ddp_7.png)

En el proyecto Node.js, cree un archivo denominado .ENV en la carpeta raíz de la aplicación y declare la variable de entorno para el ID de cliente incrustado por el PDF con el valor de la credencial API KEY del paso anterior.

```
PDF_EMBED_CLIENT_ID=**********************************************
```

Después, utilice este ID de cliente para acceder a la API de incrustación de PDF. Instale el paquete dotenv para acceder a esta variable de entorno mediante el código Node.js.

```
npm install dotenv
```

Ahora, abra el archivo app.js y agregue la siguiente línea en la parte superior del archivo para que Node.js pueda cargar el módulo dotenv:

```
require('dotenv').config();
```

## Visualización de PDF en la aplicación web

Ahora utilice la API de incrustación de PDF para mostrar PDF en el sitio. Abra la [demostración de API de incrustación de PDF](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) activa.

![Captura de pantalla de la demostración de API incrustada de PDF en vivo](assets/ddp_8.png)

En el panel izquierdo, puede elegir el modo de incrustación que mejor se adapte a las necesidades de su sitio web:

* **Ventana completa**: el PDF cubre todo el espacio de la página web

* **Contenedor de tamaño**: el PDF se muestra dentro de la página web, página por página, en una unidad de tamaño limitado

* **En línea**: todo el PDF se muestra en una div dentro de la página web

* **Lightbox**: el PDF se muestra como una capa en la parte superior de la página web

Se recomienda utilizar el modo de incrustación en línea para documentos técnicos y el generador de código más adelante para incrustar un PDF en la aplicación.

## Creación de una página en modo de incrustación en línea

Para incrustar un visor de PDF en la página web y mostrar todas las páginas simultáneamente, cree una nueva página utilizando el modo de incrustación en línea.

Cree una nueva vista en el archivo \\views\\in-line.ejs mediante el motor de visualización de EJS.

```
<! html DOCTYPE >
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/stylesheets/style.css' />
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

Y pon a tus clientes en primer lugar.

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

A continuación, modifique \\vistas\\index.ejs para crear un botón que abra la vista en línea.

```
<div class="card-body">
<h5 class="card-title">
<span>
<%= paper.title %>
</span>
</h5>
<p>
<a class="btn btn-sm btn btn-danger" href="/in-line/<%=
paper.id %>">
<span type="button"></span>
<span class="fa fa-file-pdf-o"></span>&nbsp;View Document</button>
</a>
</p>
</div>
```

Abra el archivo app.js y declare un nuevo enrutador después de la declaración indexRouter:

```
var indexRouter = require('./routes/index');
var inLineRouter = require('./routes/in-line');
```

A continuación, agregue este código después de app.use(&#39;/&#39;, indexRouter); para asociar la vista del modo de incrustación en línea con su enrutador:

```
app.use('/', indexRouter);
app.use('/in-line', inLineRouter);
```

A continuación, cree un nuevo archivo in-line.js en \\route para crear una nueva lógica de enrutador. Incluya Express, un módulo de nodo que habilita un motor de aplicación web.

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
```

A continuación, cree un punto final que gestione las solicitudes de GET de un ID de informe técnico específico y procese la vista in-line.ejs.

```
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
res.render('in-line', { title: paper.title, paper: paper });
});
module.exports = router;
```

Vuelve a ver la [demostración dinámica](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) para generar automáticamente el código de la API de incrustación del PDF. Haga clic en **En línea** en el panel izquierdo:

![Captura de pantalla de la demostración de API incrustada de PDF en vivo](assets/ddp_8.png)

Haga clic en **Generar código** para ver el código de HTML necesario para mostrar un visor de PDF de contenedor de tamaño determinado.

![Captura de pantalla de la vista previa del código](assets/ddp_9.png)

Haga clic en **Copiar código** y pegue el código en el archivo in-line.ejs.

```
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function(){
var adobeDCView = new AdobeDC.View({clientId: "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
adobeDCView.previewFile({
content:{location: {url: "https://documentcloud.adobe.com/view-sdk-demo/PDFs/Bodea Brochure.pdf"}},
metaData:{fileName: "Bodea Brochure.pdf"}
}, {embedMode: "IN_LINE"});
});
</script>
</div>
```

Sin embargo, los parámetros del documento siguen codificados. Vamos a reemplazarlos con la sintaxis de corchetes EJS (\&lt;%= someValue %\>) para representar la página según los datos del modelo del informe técnico.

```
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE"
});
});
</script>
```

Ahora ejecute la aplicación con el comando npm start y abra el sitio web en <http://localhost:3000>.

![Captura de pantalla de las miniaturas de artículo técnico de PDF](assets/ddp_10.png)

Por último, elige un artículo técnico y haz clic en **Ver documento** para abrir una nueva página con el PDF incrustado en línea:

![Captura de pantalla del informe técnico del PDF ](assets/ddp_11.png)

Observe cómo están presentes las opciones Descargar PDF y PDF de impresión.

![Captura de pantalla de las opciones de descarga e impresión](assets/ddp_12.png)

Desea controlar estos indicadores en el back-end. Más adelante puede implementar controles de autorización basados en la identidad del usuario y restringir el acceso según las reglas de su empresa. Esta complejidad no es necesaria en este caso, así que modificemos \\paths\\in-line.js para incluir las propiedades authenticated y permissions en el objeto modelo.

```
let authenticated = false;
res.render('in-line', {
title: paper.title,
paper: paper,
authenticated: authenticated,
permissions: {
showDownloadPDF: true,
showPrintPDF: true,
showFullScreen: true
}
});
```

A continuación, modifique \\views\\in-line.ejs para que la página web pueda representar los valores de indicador procedentes del motor.

```
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
Now, open the in-line.js route file and modify it to disallow the printing, downloading, and full-screen controls.
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
```

A continuación, vuelva a ejecutar la aplicación para ver cómo se refleja este cambio en el Visor del PDF.

![Captura de pantalla del archivo del PDF](assets/ddp_13.png)

## Creación de contenido cerrado

Según el escenario de usuario final, el responsable de marketing del sitio web de la empresa desea conocer mejor cómo los usuarios interactúan con su contenido basado en PDF e incorporar el contenido con el resto de su página web y marca.

Nuestro enfoque está en la incrustación de PDF, por lo que no está creando una función de autenticación de usuario. En su lugar, solo tienes que implementar un paywall simple y falso utilizando un formulario web que acepte cierta información del usuario y luego muestre el documento del PDF una vez que el usuario envíe el formulario.

Reemplace el archivo \\paths\\in-line.js por el siguiente contenido para proporcionar información de usuario al modelo de vista:

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
let authenticated = false;
let user = {};
if (req.body.firstName) {
user = {
firstName: req.body.firstName,
lastName: req.body.lastName,
jobTitle: req.body.jobTitle,
email: req.body.email,
};
authenticated = true;
}
res.render('in-line', {
title: paper.title,
paper: paper,
user: user,
authenticated: authenticated,
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
});
});
module.exports = router;
```

A continuación, sustituya el contenido de \\views\\in-line.ejs por el código siguiente. Muestra el formulario de datos de usuario o el visor del PDF, dependiendo de si se trata de un usuario autenticado.

```
<!DOCTYPE html>
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<% if (authenticated) { %>
<header class="bg-dark text-white">
<div class="text-right mr-4">Hello, <%= user.firstName %> <%= user.lastName%></div>
</header>
<% } %>
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

Y pon a tus clientes en primer lugar.

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<% if (!authenticated) { %>
<div class="row">
<form method="POST" class="center-panel text offset-md-3 col-md-6 border">
<fieldset class="offset-md-1">
<legend>Submit your info to<br/>access the whitepaper</legend>
<p><input name="firstName" placeholder="first name"/></p>
<p><input name="lastName" placeholder="last name"/></p>
<p><input name="jobTitle" placeholder="job title"/></p>
<p><input name="email" placeholder="email"/></p>
<p><button type="submit" class="btn btn-sm btn btn-primary">Submit</button></p>
</fieldset>
</form>
</div>
<% } %>
<% if (authenticated) { %>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
});
});
</script>
<% } %>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

![Captura de pantalla del contenido de control](assets/ddp_14.png)

Los visitantes del sitio ahora solo pueden acceder a los PDF después de enviar su información:

![Captura de pantalla del contenido del PDF en el visor incrustado](assets/ddp_15.png)

## Activación de eventos

Veamos cómo integrar fácilmente los eventos del visualizador del PDF con su aplicación para recopilar datos de análisis para el responsable de marketing. Para extender el visor mediante PDF EmbedAPI, agregue las siguientes líneas de código después de declarar la variable adobeDCView y antes de llamar al método previewFile:

```
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.registerCallback(
AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
function(event) {
console.log(event);
},
{ enablePDFAnalytics: true }
);
```

A continuación, vuelva a ejecutar la aplicación y abra las herramientas del desarrollador del navegador web para ver los datos del evento.

![Captura de pantalla del código](assets/ddp_16.png)

Puedes enviar estos datos a [Adobe Analytics](https://developer.adobe.com/document-services/docs/overview/pdf-embed-api) u otras herramientas de análisis.

## Pasos siguientes

Las API de [!DNL Acrobat Services] ayudan a los desarrolladores a resolver fácilmente los desafíos de la publicación digital mediante un flujo de trabajo centrado en el PDF. Ha visto cómo crear una aplicación web Node de ejemplo para mostrar una colección de documentos técnicos. Luego, adquiera una [credencial de API gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) y cree acceso restringido a los informes técnicos, que se pueden mostrar en uno de los cuatro [modos de incrustación](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf).

Agrupar este flujo de trabajo ayuda a [hipotético responsable de marketing](https://developer.adobe.com/document-services/use-cases/content-publishing/digital-content-publishing) a recopilar información de contacto a cambio de descargas de informes técnicos y ver estadísticas sobre quién interactúa con los PDF. Puede incorporar estas funciones en su sitio web para impulsar y supervisar la participación de los usuarios.

Si eres desarrollador de Angular o React, puedes probar [ejemplos adicionales](https://github.com/adobe/pdf-embed-api-samples) que muestran cómo integrar la API de PDF Embed con los proyectos de React y Angular.

Adobe te permite crear tu experiencia de cliente integral con soluciones innovadoras. Echa un vistazo a la [API Adobe PDF Embed](https://developer.adobe.com/document-services/apis/pdf-embed/) gratis. Para descubrir qué más puedes hacer, prueba la API de servicios de Adobe PDF con [pay-as-you-gopr](https://developer.adobe.com/document-services/pricing/main)[icing](https://developer.adobe.com/document-services/pricing/main).

[Empezar](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) con [!DNL Adobe Acrobat Services] API hoy mismo.
