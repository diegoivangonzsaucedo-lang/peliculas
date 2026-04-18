1️⃣ Configurar el proyecto
Asegúrate de tener Flutter instalado (flutter doctor para revisar).

Crea un proyecto nuevo si no lo tienes:

flutter create movie_review_app
Abre el proyecto en VS Code o Android Studio.

Coloca tus imágenes (Logo.png) en la carpeta assets y registra los assets en pubspec.yaml:

flutter:
  assets:
    - assets/Logo.png
2️⃣ Estructura básica de archivos
main.dart → Todo tu código actual (Splash, Login, ReviewPage)
review_page.dart (opcional para separar código)
models/review.dart (opcional para la clase Review)
3️⃣ Crear Widgets principales
SplashScreen
Mostrar logo por 3 segundos
Navegar a LoginPage
LoginPage
Formularios de email y contraseña
Botón para iniciar sesión
Validar credenciales (por ahora está hardcodeado)
Redirigir a ReviewPage
ReviewPage
Mostrar lista de reviews
Botón flotante para agregar nueva review
Mostrar estrellas según rating
Opción para cerrar sesión
4️⃣ Manejo de estado
Usa StatefulWidget para LoginPage y ReviewPage.
Para el diálogo de agregar review, usa StatefulBuilder para actualizar el slider de calificación.
5️⃣ Listas y modelos
Crea la clase Review con title, review y rating.
Mantén una lista en memoria _reviews para almacenar y mostrar las reseñas.
Al agregar una reseña, usa setState() para actualizar la lista.
6️⃣ Navegación

Usa Navigator.pushReplacement para cambiar pantallas:

Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => ReviewPage()),
);
7️⃣ Probar la app
Conecta un emulador o dispositivo físico.

Ejecuta:

flutter run
Verifica:
Splash Screen → Login → ReviewPage
Agregar nuevas reseñas
Cerrar sesión y volver a Login
