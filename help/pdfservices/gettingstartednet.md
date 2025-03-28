---
title: Introducción a la API de servicios de Adobe PDF y .Net
description: Los desarrolladores pueden empezar en solo unos minutos con los archivos de muestra listos para ejecutar que se proporcionan para acceder a todos los servicios web disponibles
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: Destacados
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---

# Introducción a la API de servicios de Adobe PDF y .Net

![Crear imagen de héroe de PDF](assets/GettingStartedJava_hero.jpg)

Los desarrolladores pueden empezar en solo unos minutos con los archivos de muestra listos para ejecutar que se proporcionan para acceder a todos los servicios web disponibles. En este tutorial se describen todos los pasos para comenzar a ejecutar los ejemplos mediante Servicios de PDF .Net SDK:

## Paso 1: Obtener credenciales y descargar archivos de muestra

El primer paso es obtener una credencial (clave de API) para desbloquear el uso. [Regístrate para obtener la prueba gratis aquí](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) y haz clic en &quot;Empezar&quot; para crear tus nuevas credenciales.

![Paso 1](assets/GettingStartedJava_step1.png)

Es importante elegir una &quot;cuenta personal&quot; para registrarse en la prueba gratis:

![Personal](assets/GettingStartedJava_personal.png)

En el siguiente paso, seleccionará el servicio de API de servicios de PDF y, a continuación, agregará un nombre y una descripción para sus credenciales.

Hay una casilla de verificación para &quot;Crear ejemplo de código personalizado&quot;. Elija esta opción para agregar automáticamente sus nuevas credenciales a sus archivos de muestra, lo que le ahorrará el paso manual de agregarlas a su proyecto.

A continuación, elija Node.js como idioma para recibir los ejemplos específicos de Node.js y haga clic en el botón &#39;Crear credenciales&#39;.

![Credenciales](assets/GettingStartedJava_credentials.png)

Recibirá un archivo .zip para descargar denominado PDFToolsSDK-.NetSamples.zip que se puede guardar en el sistema de archivos local.

## Paso 2: Configurar el entorno .Net y ejecutar el código de ejemplo

1. Descargar e instalar el SDK de [.Net](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Extraiga el archivo descargado **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** y descomprima el contenido
1. cd al directorio raíz de ejemplos **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. En el directorio raíz de ejemplos, ejecute `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>dotnet build

   Ahora está listo para ejecutar los archivos de muestra.

   Estos pasos finales le muestran cómo ejecutar su primera muestra con la operación Crear PDF desde Word:

1. En el directorio raíz de ejemplos, cambie el directorio a la carpeta CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. ejecutar `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet run CreatePDFFromDocx.csproj

El PDF se creará en la ubicación designada en la salida, que de forma predeterminada es la misma.

## Pensamientos finales

La API de servicios de PDF puede ayudarte a eliminar procesos manuales automatizando flujos de trabajo comunes y desplazando la carga de procesamiento a la nube. En un mundo en el que cada navegador trata a PDF de forma diferente, aprovechando la API Adobe PDF Embed junto con la API de servicios de PDF, puedes crear procesos optimizados, fiables y predecibles que se ejecuten y muestren correctamente **cada vez**, independientemente de la plataforma o el dispositivo.

## Recursos y pasos siguientes

* Para obtener ayuda y asistencia adicionales, visita el foro de la comunidad de [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all)

* [Documentación](https://www.adobe.com/go/pdftoolsapi_doc) de la API de servicios de PDF

* [Preguntas frecuentes](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) para preguntas de la API de servicios del PDF

* [Ponte en contacto con nosotros](https://www.adobe.com/go/pdftoolsapi_requestform) si tienes preguntas sobre licencias y precios

* Artículos relacionados

  [La nueva API de servicios de PDF ofrece aún más funciones para los flujos de trabajo de documentos](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versión de julio de [!DNL Adobe Acrobat Services]: Servicios de incrustación y PDF de PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
