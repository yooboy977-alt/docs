+++
import 'package:flutter/material.dart';
import 'package:just_audio/just_audio.dart';

void main() {
  runApp(const YooBoyMusic());
}

class YooBoyMusic extends StatelessWidget {
  const YooBoyMusic({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'YOO BOY MUSIC',
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: const Color(0xFF071426),
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      home: const MusicHome(),
    );
  }
}

class MusicHome extends StatefulWidget {
  const MusicHome({super.key});

  @override
  State<MusicHome> createState() => _MusicHomeState();
}

class _MusicHomeState extends State<MusicHome> {
  final AudioPlayer player = AudioPlayer();
  bool loading = false;

  final List<Map<String, String>> songs = [
    {
      'title': 'Wimbo wa Mfano 1',
      'artist': 'YOO BOY MUSIC',
      'url': 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3',
    },
    {
      'title': 'Wimbo wa Mfano 2',
      'artist': 'YOO BOY MUSIC',
      'url': 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3',
    },
  ];

  Future<void> playSong(String url) async {
    setState(() => loading = true);
    try {
      await player.setUrl(url);
      await player.play();
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Imeshindwa kucheza wimbo.')),
        );
      }
    } finally {
      if (mounted) setState(() => loading = false);
    }
  }

  @override
  void dispose() {
    player.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'YOO BOY MUSIC',
          style: TextStyle(fontWeight: FontWeight.bold),
        ),
        backgroundColor: const Color(0xFF102D52),
      ),
      body: ListView(
        padding: const EdgeInsets.all(18),
        children: [
          const Icon(Icons.music_note,
              size: 80, color: Colors.lightBlueAccent),
          const Text(
            'Karibu YOO BOY MUSIC',
            textAlign: TextAlign.center,
            style: TextStyle(
              fontSize: 25,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 8),
          const Text(
            'Muziki wako, sehemu moja.',
            textAlign: TextAlign.center,
          ),
          const SizedBox(height: 28),
          const Text(
            'Nyimbo mpya',
            style: TextStyle(fontSize: 21),
          ),
          const SizedBox(height: 10),
          for (final song in songs)
            Card(
              child: ListTile(
                leading: const Icon(
                  Icons.album,
                  color: Colors.lightBlueAccent,
                  size: 38,
                ),
                title: Text(song['title']!),
                subtitle: Text(song['artist']!),
                trailing: IconButton(
                  icon: const Icon(Icons.play_circle_fill),
                  iconSize: 35,
                  onPressed: loading
                      ? null
                      : () => playSong(song['url']!),
                ),
              ),
            ),
          const SizedBox(height: 20),
          StreamBuilder<PlayerState>(
            stream: player.playerStateStream,
            builder: (context, snapshot) {
              final playing = snapshot.data?.playing ?? false;
              return ElevatedButton.icon(
                onPressed: () {
                  if (playing) {
                    player.pause();
                  } else {
                    player.play();
                  }
                },
                icon: Icon(
                  playing ? Icons.pause : Icons.play_arrow,
                ),
                label: Text(playing ? 'Simamisha' : 'Endelea kucheza'),
              );
            },
          ),
        ],
      ),
    );
  }
}

title="For App Developers"
weight=1
expand=true
include_summaries=true
+++
