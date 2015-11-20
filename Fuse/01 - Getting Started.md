# $(Iniciando)

Comenzar y correr Fuse es súper fácil. Solo tendremos que instalar algunas cosas, y estaremos listos para iniciar!

## $(Setup)

Siempre estamos trabajando duro para hacer que este proceso se suave como la seda, pero de vez en cuando las cosas todavía pueden ir mal. Si sucede algo, por favor háganoslo saber.
Instrucciones para instalar en OS X: https://www.fusetools.com/developers/guides/setup/install-osx

Instrucciones para desinstalar en OS X: https://gist.github.com/Tapped/daa78c08882f33b0c7c3

Instrucciones para instalar en Windows: https://www.fusetools.com/developers/guides/setup/install-win

## Tutorial

Sugerimos que inicie [revisando nuestro tutorial](https://www.fusetools.com/developers/guides/tutorial). También puede encontrar una variedad de ejemplos listos para ejecutar en [nuestra página de Ejemplos](https://www.fusetools.com/examples), así como tambien en nuestra lista (en constante crecimiento) de [tutoriales en Youtube](https://www.youtube.com/playlist?list=PLdlqWm6b-XALJgM3fGa4q95Yipsgb8Q1o).

## $(Estructura del Proyecto)

Después de haber creado un nuevo proyecto, ya sea usando el dashboard  o el comando `fuse` desde la línea de comandos, encontrará dos archivos en el directorio del proyecto:

- `ProjectName.unoproj` - Este es el archivo de proyecto, y, básicamente, hace un seguimiento de los archivos que forman parte del proyecto, los paquetes de los que depende, así como también otros valores prácticos como las api keys.
- `MainView.ux` - Este es el punto inicio principal para su app, principalmente por que contiene el tag `App`. Bajo circunstancias normales usted puede borrar la mayoria de contenido de este archivo, pero sientase libre de examinar la aplicación por defecto y mire el esqueleto con algunos controles.

Nota: No es necesita hacer referencia a JavaScript desde el archivo `unoproj`. Los archivos JavaScript son referenciados directamente desde la UX.

> ### $(Estructura de archivos del proyecto)

El archivo `unoproj`  tiene la siguiente estructura (incompleto):

Divide the solution into multiple projects and reference them from the `unoproj`-file:

```
"Projects" : [
	"path_to_other_project.unoproj"
]
```

__Note__: This section of the documentation is incomplete, a full description of the `unoproj` file is coming.



> ### $(Sublime Text projects)

When you drop a folder into Sublime Text 3, it will by default search through all files in all subfolders. When building a Fuse project, this isn't always what you want.

If you create a file called `ProjectName.sublime-project`, you can drop this into it to make it ignore the `.cache` and `.build` directories:

```
{
	"folders":
	[
		{
			"folder_exclude_patterns":
			[
				".build",
				".cache",
			],
			"path": "."
		}
	]
}
```

This file can then be opened from the `Project` -> `Open Project...`-dialog.

> ### Git

If you are using Git for version control, you can put the following in your .gitignore file.

	.build
	.cache

## Preview

Live preview is a key feature of Fuse. You can live preview simultaneously on multiple devices (and in the desktop simulator), so you no longer need to build your project for specific devices between edits; just save and they will appear instantly on all of your devices.

For instructions on how to enable iOS preview, go here: https://www.fusetools.com/developers/guides/previewandexport/devicepreview

For instructions on how to enable preview on an Android device, go here: https://www.fusetools.com/developers/guides/previewandexport/devicepreview

Starting a preview of your project on your desktop can be done in a couple of ways. If you have Sublime Text 3 installed with the supported plugin, you can right click on the UX-file and select "Begin Fuse preview" and select the "Local" option. Preview will then open the simulator, and it will sync automatically as you edit your files.

Alternatively, you can navigate on the command line to your project root directory and type `fuse preview`.

You can read more about starting Fuse preview on the desktop here: https://www.fusetools.com/developers/guides/previewandexport/toolpreview

## Export

When running preview, Fuse will create a shell application on your device that connects to the Fuse daemon running on your system to quickly display changes you make to the project. However, if you want to bring the device with you to a meeting or to show people away from your development environment, you will need to export the project to the device you are targeting.

> ### Exporting to iOS

On iOS, you need to have a machine running OS X and Xcode installed (as for preview). You also need an Apple developer account. In the project root, simply type:

`fuse build --target=iOS --run`

This will open the built project in Xcode. If you select your device from the pulldown list of available output targets and press the "run" button, the application will deploy to your device and run.

> ### Exporting to Android

To export to Android, you go to the project root and type:

`fuse build --target=Android --run`

This will deploy and start the project on your connected Android device.
