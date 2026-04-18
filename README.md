# peliculas
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(MovieReviewApp());
}

class MovieReviewApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Reseñas de Películas',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SplashScreen(),
    );
  }
}

////////////////////////////////////////////////////////////
/// 🔥 SPLASH SCREEN
////////////////////////////////////////////////////////////
class SplashScreen extends StatefulWidget {
  @override
  _SplashScreenState createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen> {
  @override
  void initState() {
    super.initState();

    Future.delayed(Duration(seconds: 3), () {
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => LoginPage()),
      );
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Image.asset(
          'assets/Logo.png',
          width: 200,
        ),
      ),
    );
  }
}

////////////////////////////////////////////////////////////
/// LOGIN
////////////////////////////////////////////////////////////
class LoginPage extends StatefulWidget {
  @override
  _LoginPageState createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final emailController = TextEditingController();
  final passwordController = TextEditingController();

  String savedEmail = "test@gmail.com";
  String savedPassword = "1234";

  void login() {
    if (emailController.text == savedEmail &&
        passwordController.text == savedPassword) {

      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => ReviewPage()),
      );

    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Credenciales incorrectas")),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Login')),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Image.asset('assets/Logo.png', width: 120),
            SizedBox(height: 20),
            TextField(
              controller: emailController,
              decoration: InputDecoration(labelText: "Email"),
            ),
            TextField(
              controller: passwordController,
              decoration: InputDecoration(labelText: "Contraseña"),
              obscureText: true,
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: login,
              child: Text("Iniciar sesión"),
            ),
          ],
        ),
      ),
    );
  }
}

////////////////////////////////////////////////////////////
/// MODELO
////////////////////////////////////////////////////////////
class Review {
  final String title;
  final String review;
  final int rating;

  Review({
    required this.title,
    required this.review,
    required this.rating,
  });
}

////////////////////////////////////////////////////////////
/// REVIEWS PAGE (CON API)
////////////////////////////////////////////////////////////

class ReviewPage extends StatefulWidget {
  @override
  _ReviewPageState createState() => _ReviewPageState();
}


class _ReviewPageState extends State<ReviewPage> {
  final List<Review> _reviews = [
    Review(
      title: "Avengers: Endgame",
      review: "Los héroes intentan revertir el chasquido de Thanos en una batalla épica.",
      rating: 5,
    ),
    Review(
      title: "Titanic",
      review: "Historia de amor entre Jack y Rose en el trágico hundimiento del barco.",
      rating: 5,
    ),
    Review(
      title: "Spider-Man: No Way Home",
      review: "Spider-Man enfrenta villanos de otros universos en una gran aventura.",
      rating: 4,
    ),
    Review(
      title: "Batman: The Dark Knight",
      review: "El Joker desata el caos en Gotham en una de las mejores películas de superhéroes.",
      rating: 5,
    ),
    Review(
      title: "Interstellar",
      review: "Viaje espacial para salvar la humanidad a través de agujeros de gusano.",
      rating: 5,
    ),
    Review(
      title: "Joker",
      review: "La transformación de Arthur Fleck en el villano Joker.",
      rating: 5,
    ),
    Review(
      title: "Dragon ball",
      review: "e las mejores películas de superhéroes.",
      rating: 5,
    ),
  ];

  final TextEditingController _titleController = TextEditingController();
  final TextEditingController _reviewController = TextEditingController();
  int _rating = 3;

  void _addReview() {
    if (_titleController.text.isEmpty || _reviewController.text.isEmpty) return;

    setState(() {
      _reviews.add(
        Review(
          title: _titleController.text,
          review: _reviewController.text,
          rating: _rating,
        ),
      );
    });

    _titleController.clear();
    _reviewController.clear();
    _rating = 3;
  }

  Widget _buildStars(int rating) {
    return Row(
      children: List.generate(
        5,
        (index) => Icon(
          index < rating ? Icons.star : Icons.star_border,
          color: Colors.amber,
        ),
      ),
    );
  }

  void _showAddReviewDialog() {
    showDialog(
      context: context,
      builder: (context) {
        int tempRating = _rating;

        return StatefulBuilder(
          builder: (context, setModalState) {
            return AlertDialog(
              title: Text("Agregar Reseña"),
              content: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  TextField(
                    controller: _titleController,
                    decoration: InputDecoration(labelText: "Película"),
                  ),
                  TextField(
                    controller: _reviewController,
                    decoration: InputDecoration(labelText: "Reseña"),
                  ),
                  Text("Calificación: $tempRating"),
                  Slider(
                    value: tempRating.toDouble(),
                    min: 1,
                    max: 5,
                    divisions: 4,
                    onChanged: (value) {
                      setModalState(() {
                        tempRating = value.toInt();
                      });
                    },
                  ),
                ],
              ),
              actions: [
                TextButton(
                  onPressed: () => Navigator.pop(context),
                  child: Text("Cancelar"),
                ),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _rating = tempRating;
                    });
                    _addReview();
                    Navigator.pop(context);
                  },
                  child: Text("Guardar"),
                ),
              ],
            );
          },
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("🎬 Netflix Reviews"),
        actions: [
          IconButton(
            icon: Icon(Icons.logout),
            onPressed: () {
              Navigator.pushReplacement(
                context,
                MaterialPageRoute(builder: (context) => LoginPage()),
              );
            },
          )
        ],
      ),
      body: ListView.builder(
        itemCount: _reviews.length,
        itemBuilder: (context, index) {
          final review = _reviews[index];

          return Card(
            margin: EdgeInsets.all(10),
            child: ListTile(
              title: Text(review.title),
              subtitle: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(review.review),
                  SizedBox(height: 5),
                  _buildStars(review.rating),
                ],
              ),
            ),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _showAddReviewDialog,
        child: Icon(Icons.add),
      ),
    );
  }
}
