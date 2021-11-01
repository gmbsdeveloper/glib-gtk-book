# ¿Por qué y cuándo crear subclases de widgets GTK?

Si buscamos, por ejemplo, en \lstinline{GeditTab}, contiene --- por composición --- a \lstinline{GeditView}. \lstinline{GeditView} es una subclase de \lstinline{GtkSourceView}. En su lugar, \lstinline{GeditTab} podría usar por composición directamente un objeto \lstinline{GtkSourceView} y mover el código de \lstinline{GeditView} a \lstinline{GeditTab}. Pero, por lo general, sucede o debería ocurrir exactamente lo contrario.

Cuando la base de código de una aplicación GTK aún es pequeña, por ejemplo, si comienza a escribir un equivalente de \lstinline{GeditTab}, puede crear un objeto \lstinline{GtkSourceView} directamente en \lstinline{GeditTab} y almacenar la \lstinline{GtkSourceView} objeto en una variable de instancia. Luego, al implementar nuevas características, agrega nuevas funciones que usan casi exclusivamente la variable de instancia \lstinline{GtkSourceView}. Incluso puede tener funciones \lstinline{static} que toman directamente un argumento \lstinline{GtkSourceView} en lugar del parámetro \lstinline{GeditTab} \emph{self}. También puede almacenar datos adicionales útiles solo para las funciones relacionadas con \lstinline{GtkSourceView}. Si la clase \lstinline{GeditTab} aún es pequeña (por ejemplo, 500 líneas de código) y no contiene muchas variables de instancia, no hay problema. Por otro lado, si la clase \lstinline{GeditTab} se vuelve más grande (por ejemplo, más de 2000 líneas de código), entonces probablemente sea una señal de que la clase debería delegar parte de su trabajo a una nueva clase; en nuestro caso, \lstinline{GeditView}. Tenga en cuenta que 2000 líneas de código para una clase pueden estar bien, no hay un límite claro sobre cuándo se debe dividir una clase. Pero si la clase \lstinline{GeditView} resultante contendría al menos varios cientos de código no estándar, probablemente sea una buena idea hacer la refactorización.

De lo que se trata OOP es de empaquetar datos y comportamiento juntos, y delegar parte del trabajo a otras clases. La herencia de clases tiene sentido cuando queremos agregar más comportamiento a una clase existente, con posibles datos adicionales relacionados con el comportamiento agregado. \lstinline{GeditView} es una subclase de \lstinline{GtkSourceView} porque \lstinline{GeditView} \emph{es~a} \lstinline{GtkSourceView}; es decir, \lstinline{GeditView} opera con los mismos datos base que \lstinline{GtkSourceView}. Además, permite a \lstinline{GeditTab} delegar parte de su trabajo, con el objetivo de tener clases más pequeñas y manejables. Más pequeño de dos formas: menos código y menos variables de instancia.

Por lo tanto, durante la vida útil de una aplicación GTK, el programador a menudo necesita refactorizar el código, crear nuevas clases y delegar más trabajo. Lo contrario puede suceder cuando el código de la aplicación se mueve a la biblioteca subyacente; por ejemplo, si todas las características de \lstinline{GeditView} se agregan a la clase \lstinline{GtkSourceView}; en ese caso, la subclase \lstinline{GeditView} ya no tiene sentido.