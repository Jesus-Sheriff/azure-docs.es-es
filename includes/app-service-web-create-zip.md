## <a name="create-a-project-zip-file"></a>Creación de un archivo ZIP de proyecto

Asegúrese de que todavía se encuentra en el directorio raíz del proyecto de ejemplo. Cree un archivo ZIP con todo el contenido del proyecto. El siguiente comando usa la herramienta predeterminada de su terminal:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

Posteriormente, cargue el archivo ZIP en Azure e impleméntelo en App Service.