# Deploy Service on Linux Local Cluster

* Antes de empezar, recuerda haber instalado y configurado [Visual Studio] para manejar carga de trabajo ASP.NET y de Desarrollo Web

## Instalación y configuración de WSL

* Abra PowerShell para Habilitar Windows Subsystem for Linux e ingrese este comando

~~~
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
~~~

* Habilite la función Virtual Machine
~~~
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
~~~

* Descargue e instale el [Paquete de Actualización] del kernel de Linux
* Configure WSL 2 como la versión por defecto
~~~
wsl --set-default-version 2
~~~
* Instale la versión 18.04 de Ubuntu desde la [Microsoft Store]

## Instalación y configuración de Docker Desktop

* Descargue e instale [Docker Desktop] para Windows. 
Durante la Instalación, asegúrese de seleccionar WSL 2 en vez de Hyper-V.

* Una vez instalado, inicie Docker Desktop y manténgalo en segundo plano.
* Al costado derecho de la barra de tareas asegúrese de que esté utilizando Linux Containers. De lo contrario, haga click en **Switch to Linux Containers**

## Creación de un contenedor local y configuración de Service Fabric
* Actualice la configuración de Docker Daemon en su Host con el siguiente comando
~~~
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8:1::/64"
}
~~~

* Inicie el Cluster via WSL Powershell
~~~
docker run --name sftestcluster -d -v /var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mcr.microsoft.com/service-fabric/onebox:u18
~~~

* Cuando termine de utilizar el cluster, detenga y limpie el contenedor con el siguiente comando.
~~~
docker rm -f sftestcluster
~~~

## Creación y Despliegue de Servicio

* Abra Visual Studio y seleccione la opción Crear un Proyecto
* Seleccione el tipo de aplicación ASP .NET Core Web API
* Habilite las opciones de HTTPS y Docker
* Seleccione Linux como sistema operativo para Docker
* Deje activas las opciones Usar Controladores y habilite la compatibilidad con OpenAPI.
* Una vez creado el proyecto, para ejecutarlo presione la tecla F5
* Al aparecer un mensaje emergente presione Si, para confiar en el Certificado SSL y se desplegará el servicio con Docker

[Visual Studio]: https://visualstudio.microsoft.com/es/vs/community/
[Paquete de Actualización]: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
[Microsoft Store]: https://apps.microsoft.com/store/detail/ubuntu-1804-lts/9N9TNGVNDL3Q?hl=es-es&gl=ES
[Docker Desktop]: https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe
