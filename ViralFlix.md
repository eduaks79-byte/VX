# VX
import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';
import '../widgets/video_player.dart';

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      body: StreamBuilder(
        stream: FirebaseFirestore.instance
            .collection("posts")
            .orderBy("impulsionado", descending: true)
            .orderBy("createdAt", descending: true)
            .snapshots(),
        builder: (context, snapshot) {
          if (!snapshot.hasData)
            return Center(child: CircularProgressIndicator());

          final posts = snapshot.data!.docs;

          return PageView.builder(
            scrollDirection: Axis.vertical,
            itemCount: posts.length,
            itemBuilder: (context, index) {
              final post = posts[index];

              return Stack(
                children: [
                  VideoPlayerWidget(url: post['mediaUrl']),

                  Positioned(
                    right: 10,
                    bottom: 100,
                    child: Column(
                      children: [
                        Icon(Icons.favorite, color: Colors.white),
                        SizedBox(height: 20),
                        ElevatedButton(
                          onPressed: () {},
                          child: Text("Impulsionar 🚀"),
                        ),
                      ],
                    ),
                  )
                ],
              );
            },
          );
        },
      ),
    );
  }
}
