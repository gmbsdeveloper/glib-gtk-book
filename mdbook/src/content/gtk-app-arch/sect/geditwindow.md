# \section{GeditWindow}

\lstinline{GeditWindow} es una subclase de \lstinline{GtkApplicationWindow}. Y no lo vemos en el esquema, pero \lstinline{GtkApplicationWindow} es una subclase de \lstinline{GtkWindow}, que es un widget de nivel superior. Un widget de nivel superior no puede incluirse en otro widget. Una \lstinline{GtkApplicationWindow} está contenida en una \lstinline{GtkApplication}, pero \lstinline{GtkApplication} no es una subclase de \lstinline{GtkWidget}.

En el esquema, la notación ``\texttt{1}'' y ``\texttt{1..*} '' significa que un objeto \lstinline{GeditApp} \emph{contiene} uno o varios \lstinline{GeditWindow} objetos, y que una \lstinline{GeditWindow} está contenida exactamente en una \lstinline{GeditApp} (una \lstinline{GeditWindow} no puede estar contenida en varios objetos \lstinline{GeditApp}, de todos modos solo hay una instancia de \lstinline{GeditApp} por proceso).

\lstinline{GeditWindow} es responsable de crear la interfaz de usuario principal, creando otros widgets y ensamblándolos en un contenedor \lstinline{GtkGrid}, por ejemplo. Otra cosa que hace \lstinline{GeditWindow} es implementar las \lstinline{GActions} que tienen un efecto solo en la ventana actual, por ejemplo, una acción para cerrar la ventana o guardar el documento actual. Al implementar una \lstinline{GAction}, \lstinline{GeditWindow} puede, por supuesto, delegar la mayor parte de su trabajo a otras clases contenidas en \lstinline{GeditWindow}.

En la parte superior de la ventana principal de una aplicación, generalmente hay una \lstinline{GtkHeaderBar}, que muestra el título de la ventana, algunos botones y un menú de ``hamburguesa''. Alternativamente, una aplicación puede tener una barra de menú y una barra de herramientas tradicionales.

Además de la barra de encabezado, \lstinline{GeditWindow} crea un widget \lstinline{GeditStatusbar} y lo agrega al final de la ventana. También crea dos \lstinline{GeditPanels}, uno en el lado izquierdo de la ventana y el otro en la parte inferior, encima de la \lstinline{GeditStatusbar}. Cada panel puede contener varios elementos. Por ejemplo, el panel lateral contiene un explorador de archivos integrado y el panel inferior puede contener una terminal, entre otras cosas \footnote{El código gedit actual en realidad ya no contiene una clase \lstinline{GeditPanel}, pero fue el caso en una versión anterior. Se agregó \lstinline{GeditPanel} al diagrama para mostrar una posible implementación de paneles en una aplicación. Si su aplicación contiene solo un elemento en un panel, no es necesario tener una clase \lstinline{Panel}, puede agregar directamente el elemento a la ventana.}.

\lstinline{GeditWindow} también crea un \lstinline{GeditNotebook}, la parte principal de la ventana.