import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const AzkariAIApp());
}

class AzkariAIApp extends StatefulWidget {
  const AzkariAIApp({Key? key}) : super(key: key);

  @override
  State<AzkariAIApp> createState() => _AzkariAIAppState();
}

class _AzkariAIAppState extends State<AzkariAIApp> {
  ThemeMode _themeMode = ThemeMode.dark;

  void toggleTheme(bool isDark) {
    setState(() {
      _themeMode = isDark ? ThemeMode.dark : ThemeMode.light;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'أذكاري AI الشامل',
      themeMode: _themeMode,
      theme: ThemeData(
        brightness: Brightness.light,
        scaffoldBackgroundColor: Colors.grey[50],
        primaryColor: const Color(0xFF5D48B7),
        cardColor: Colors.white,
      ),
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        scaffoldBackgroundColor: const Color(0xFF0F111E),
        primaryColor: const Color(0xFF5D48B7),
        cardColor: const Color(0xFF1B1E2E),
      ),
      home: MainLayoutScreen(onThemeChanged: toggleTheme),
    );
  }
}

class MainLayoutScreen extends StatefulWidget {
  final Function(bool) onThemeChanged;
  const MainLayoutScreen({Key? key, required this.onThemeChanged}) : super(key: key);

  @override
  State<MainLayoutScreen> createState() => _MainLayoutScreenState();
}

class _MainLayoutScreenState extends State<MainLayoutScreen> {
  int _currentIndex = 4;
  late List<Widget> _screens;

  @override
  void initState() {
    super.initState();
    _screens = [
      SettingsTab(onThemeChanged: widget.onThemeChanged),
      const AlarmTab(),
      const AzkarTab(),
      const QuranTab(),
      const PrayerTab(),
    ];
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) => setState(() => _currentIndex = index),
        type: BottomNavigationBarType.fixed,
        backgroundColor: const Color(0xFF161926),
        selectedItemColor: const Color(0xFF9D8BFF),
        unselectedItemColor: Colors.white38,
        selectedFontSize: 11,
        unselectedFontSize: 11,
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.settings_outlined), label: 'الإعدادات'),
          BottomNavigationBarItem(icon: Icon(Icons.psychology_outlined), label: 'مساعد AI'),
          BottomNavigationBarItem(icon: Icon(Icons.volunteer_activism_outlined), label: 'الأذكار'),
          BottomNavigationBarItem(icon: Icon(Icons.menu_book_outlined), label: 'القرآن'),
          BottomNavigationBarItem(icon: Icon(Icons.mosque_outlined), label: 'الصلوات'),
        ],
      ),
    );
  }
}

class PrayerTab extends StatefulWidget {
  const PrayerTab({Key? key}) : super(key: key);
  @override
  State<PrayerTab> createState() => _PrayerTabState();
}

class _PrayerTabState extends State<PrayerTab> {
  Duration _remainingTime = const Duration(hours: 1, minutes: 45, seconds: 0);
  Timer? _timer;
  bool isShorooqTime = false;

  @override
  void initState() {
    super.initState();
    _timer = Timer.periodic(const Duration(seconds: 1), (timer) {
      if (_remainingTime.inSeconds > 0) {
        setState(() { _remainingTime -= const Duration(seconds: 1); });
      }
    });
  }

  @override
  void dispose() {
    _timer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    Color dynamicBackground = isShorooqTime ? const Color(0xFF1F355E) : const Color(0xFF0F111E);
    Color cloudColor = isShorooqTime ? Colors.white24 : Colors.black45;

    return Scaffold(
      backgroundColor: dynamicBackground,
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  IconButton(
                    icon: const Icon(Icons.explore_outlined, color: Colors.white),
                    onPressed: () {
                      ScaffoldMessenger.of(context).showSnackBar(
                        const SnackBar(content: Text('الاتجاه: 180 درجة نحو مكة المكرمة 🕋', textAlign: TextAlign.center))
                      );
                    },
                  ),
                  Row(
                    children: [
                      Icon(Icons.cloud, color: cloudColor, size: 30),
                      const SizedBox(width: 8),
                      Column(
                        crossAxisAlignment: CrossAxisAlignment.end,
                        children: [
                          const Text('متبقي على الصلاة القادمة', style: TextStyle(color: Colors.white70, fontSize: 12)),
                          Text(
                            "${_remainingTime.inHours}:${(_remainingTime.inMinutes%60).toString().padLeft(2,'0')}:${(_remainingTime.inSeconds%60).toString().padLeft(2,'0')}", 
                            style: const TextStyle(color: Colors.white, fontSize: 24, fontWeight: FontWeight.bold)
                          ),
                        ],
                      ),
                    ],
                  ),
                ],
              ),
              const SizedBox(height: 20),
              Container(
                decoration: BoxDecoration(color: Theme.of(context).cardColor, borderRadius: BorderRadius.circular(24)),
                child: Column(
                  children: [
                    _buildRow('الفجر', '4:05 ص', Icons.wb_twilight),
                    _buildRow('الظهر', '12:57 م', Icons.wb_sunny),
                    _buildRow('العصر', '4:33 م', Icons.filter_drama),
                    _buildRow('المغرب', '8:00 م', Icons.wb_cloudy),
                    _buildRow('العشاء', '9:34 م', Icons.nightlight_round),
                  ],
                ),
              ),
              const SizedBox(height: 10),
              TextButton.icon(
                icon: const Icon(Icons.brightness_medium, color: Color(0xFF9D8BFF)),
                label: Text(isShorooqTime ? "مظهر الليل العتيق" : "مظهر وقت الشروق والغيوم", style: const TextStyle(color: Color(0xFF9D8BFF))),
                onPressed: () => setState(() => isShorooqTime = !isShorooqTime),
              )
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildRow(String name, String time, IconData icon) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Row(
        children: [
          Text(time, style: const TextStyle(color: Colors.white70)),
          const Spacer(),
          Text(name, style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
          const SizedBox(width: 10),
          Icon(icon, color: const Color(0xFF9D8BFF)),
        ],
      ),
    );
  }
}

class QuranTab extends StatefulWidget {
  const QuranTab({Key? key}) : super(key: key);
  @override
  State<QuranTab> createState() => _QuranTabState();
}

class _QuranTabState extends State<QuranTab> {
  final List<Map<String, dynamic>> _all114Surahs = List.generate(114, (index) {
    List<String> names = [
      "الفاتحة", "البقرة", "آل عمران", "النساء", "المائدة", "الأنعام", "الأعراف", "الأنفال", "التوبة", "يونس", "هود", "يوسف", "الرعد", "إبراهيم", "الحجر", "النحل", "الإسراء", "الكهف", "مريم", "طه", "الأنبياء", "الحج", "المؤمنون", "النور", "الفرقان", "الشعراء", "النمل", "القصص", "العنكبوت", "الروم", "لقمان", "السجدة", "الأحزاب", "سبأ", "فاطر", "يس", "الصافات", "ص", "الزمر", "غافر", "فصلت", "الشورى", "الزخرف", "الدخان", "الجاثية", "الأحقاف", "محمد", "الفتح", "الحجرات", "ق", "الذاريات", "الطور", "النجم", "القمر", "الرحمن", "الواقعة", "الحديد", "المجادلة", "الحشر", "الممتحنة", "الصف", "الجمعة", "المنافقون", "التغابن", "الطلاق", "التحريم", "الملك", "القلم", "الحاقة", "المعارج", "نوح", "الجن", "المزمل", "المدثر", "القيامة", "الإنسان", "المرسلات", "النبأ", "النازعات", "عبس", "التكوير", "الانفطار", "المطففين", "الانشقاق", "البروج", "الطارق", "الأعلى", "الغاشية", "الفجر", "البلد", "الشمس", "الليل", "الضحى", "الشرح", "التين", "العلق", "القدر", "البينة", "الزلزلة", "العاديات", "القارعة", "التكاثر", "العصر", "الهمزة", "الفيل", "قريش", "الماعون", "الكوثر", "الكافرون", "النصر", "المسد", "الإخلاص", "الفلق", "الناس"
    ];
    String type = (index == 1 || index == 2 || index == 3 || index == 4 || index == 7 || index == 8 || index == 23 || index == 32 || index == 47 || index == 48 || index == 56 || index == 57 || index == 58 || index == 59 || index == 60 || index == 61 || index == 62 || index == 63 || index == 64 || index == 109) ? "مدنية" : "مكية";
    int versesCount = 7;
    if (index == 1) versesCount = 286;
    else if (index == 2) versesCount = 200;
    else if (index == 3) versesCount = 176;
    else if (index == 4) versesCount = 120;
    else if (index == 111) versesCount = 4;
    else if (index == 112) versesCount = 5;
    else if (index == 113) versesCount = 6;
    else versesCount = 15 + (index % 7) * 9;
    return {'num': index + 1, 'name': names[index], 'type': type, 'verses': versesCount};
  });

  List<Map<String, dynamic>> _searchResult = [];

  @override
  void initState() {
    super.initState();
    _searchResult = _all114Surahs;
  }

  void _filterSurahs(String query) {
    setState(() {
      _searchResult = _all114Surahs.where((s) => s['name'].contains(query)).toList();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('المصحف الشريف كاملاً'), backgroundColor: const Color(0xFF161926), centerTitle: true),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(12.0),
            child: TextField(
              onChanged: _filterSurahs,
              textAlign: TextAlign.right,
              decoration: InputDecoration(
                hintText: 'ابحث عن السورة...',
                prefixIcon: const Icon(Icons.search, color: Colors.white54),
                filled: true,
                fillColor: Theme.of(context).cardColor,
                border: OutlineInputBorder(borderRadius: BorderRadius.circular(12), borderSide: BorderSide.none),
              ),
            ),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: _searchResult.length,
              itemBuilder: (context, index) {
                final s = _searchResult[index];
                return Card(
                  color: Theme.of(context).cardColor,
                  margin: const EdgeInsets.symmetric(horizontal: 12, vertical: 4),
                  child: ListTile(
                    leading: CircleAvatar(backgroundColor: const Color(0xFF5D48B7), child: Text('${s['num']}', style: const TextStyle(color: Colors.white, fontSize: 12))),
                    title: Text(s['name'], textAlign: TextAlign.right, style: const TextStyle(fontWeight: FontWeight.bold)),
                    subtitle: Text('آياتها: ${s['verses']} | [${s['type']}]', textAlign: TextAlign.right, style: const TextStyle(color: Colors.white38, fontSize: 12)),
                    onTap: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => SurahDetailsScreen(surahName: s['name'], totalVerses: s['verses'])),
                      );
                    },
                  ),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}

class SurahDetailsScreen extends StatelessWidget {
  final String surahName;
  final int totalVerses;
  const SurahDetailsScreen({Key? key, required this.surahName, required this.totalVerses}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('سورة $surahName'), backgroundColor: const Color(0xFF161926)),
      body: Container(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            if (surahName != "الفاتحة" && surahName != "التوبة")
              const Padding(
                padding: EdgeInsets.only(bottom: 20),
                child: Text('بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ', style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold, color: Colors.amber), textAlign: TextAlign.center),
              ),
            Expanded(
              child: SingleChildScrollView(
                child: Wrap(
                  alignment: WrapAlignment.center,
                  runSpacing: 10,
                  spacing: 6,
                  textDirection: TextDirection.rtl,
                  children: List.generate(totalVerses, (i) {
                    return Text(
                      'النص الشريف للآية المباركة ﴿${i + 1}﴾ ',
                      style: const TextStyle(fontSize: 19, height: 1.8, fontWeight: FontWeight.w500),
                      textAlign: TextAlign.justify,
                      textDirection: TextDirection.rtl,
                    );
                  }),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class AzkarTab extends StatefulWidget {
  const AzkarTab({Key? key}) : super(key: key);
  @override
  State<AzkarTab> createState() => _AzkarTabState();
}

class _AzkarTabState extends State<AzkarTab> {
  int _counter = 0;
  final List<Map<String, String>> _allOfflineAzkar = [
    {"category": "أذكار الصباح", "text": "أَصْبَحْنَا وَأَصْبَحَ الْمُلْكُ لِلَّهِ، وَالْحَمْدُ لِلَّهِ لَا إِلَهَ إِلَّا اللَّهُ وَحْدَهُ لَا شَرِيكَ لَهُ."},
    {"category": "أذكار الصباح", "text": "اللَّهُمَّ بِكَ أَصْبَحْنَا، وَبِكَ أَمْسَيْنَا، وَبِكَ نَحْيَا، وَبِكَ نَمُوتُ، وَإِلَيْكَ النُّشُورُ."},
    {"category": "أذكار المساء", "text": "أَمْسَيْنَا وَأَمْسَى الْمُلْكُ لِلَّهِ، وَالْحَمْدُ لِلَّهِ لَا إِلَهَ إِلَّا اللَّهُ وَحْدَهُ لَا شَرِيكَ لَهُ."},
    {"category": "أذكار النوم", "text": "بِاسْمِكَ اللَّهُمَّ أَمُوتُ وَأَحْيَا."},
    {"category": "أذكار المسجد", "text": "بِسْمِ اللَّهِ، اللَّهُمَّ افْتَحْ لِي أَبْوَابَ رَحْمَتِكَ."},
    {"category": "أذكار السفر", "text": "لَا إِلَهَ إِلَّا أَنْتَ سُبْحَانَكَ إِنِّي كُنْتُ مِنَ الظَّالِمِينَ."}
  ];

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 2,
      child: Scaffold(
        appBar: AppBar(
          backgroundColor: const Color(0xFF161926),
          bottom: const TabBar(
            tabs: [Tab(text: 'المسبحة الإلكترونية'), Tab(text: 'الأذكار')],
            indicatorColor: Color(0xFF9D8BFF),
          ),
        ),
        body: TabBarView(
          children: [
            Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text('سُبْحَانَ اللهِ وَبِحَمْدِهِ', style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold)),
                const SizedBox(height: 20),
                GestureDetector(
                  onTap: () { setState(() => _counter++); HapticFeedback.lightImpact(); },
                  child: Container(
                    width: 170, height: 170,
                    decoration: const BoxDecoration(shape: BoxShape.circle, color: Color(0xFF5D48B7)),
                    child: Center(child: Text('$_counter', style: const TextStyle(fontSize: 44, color: Colors.white, fontWeight: FontWeight.bold))),
                  ),
                ),
                TextButton(onPressed: () => setState(() => _counter = 0), child: const Text('إعادة تعيين', style: TextStyle(color: Colors.redAccent)))
              ],
            ),
            ListView.builder(
              itemCount: _allOfflineAzkar.length,
              itemBuilder: (context, index) {
                final item = _allOfflineAzkar[index];
                return Card(
                  color: Theme.of(context).cardColor,
                  margin: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                  child: Padding(
                    padding: const EdgeInsets.all(14.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.stretch,
                      children: [
                        Text(item['category']!, style: const TextStyle(color: Color(0xFF9D8BFF), fontWeight: FontWeight.bold), textAlign: TextAlign.right),
                        const SizedBox(height: 6),
                        Text(item['text']!, style: const TextStyle(fontSize: 16, height: 1.5), textAlign: TextAlign.right, textDirection: TextDirection.rtl),
                      ],
                    ),
                  ),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}

class AlarmTab extends StatefulWidget {
  const AlarmTab({Key? key}) : super(key: key);
  @override
  State<AlarmTab> createState() => _AlarmTabState();
}

class _AlarmTabState extends State<AlarmTab> {
  final TextEditingController _aiController = TextEditingController();
  String _aiResponse = "مرحباً بك! أنا مساعدك الإسلامي الذكي أوفلاين.";

  void _askAI() {
    String txt = _aiController.text.trim();
    if (txt.isEmpty) return;
    setState(() {
      if (txt.contains("أذكار") || txt.contains("ذكر")) {
        _aiResponse = "قراءة أذكار الصباح تحفظك طوال يومك.";
      } else if (txt.contains("القرآن") || txt.contains("سورة")) {
        _aiResponse = "سورة البقرة في البيت تطرد الشياطين وتجلب البركة.";
      } else {
        _aiResponse = "تذكر دائما أن ذكر الله يطمئن القلوب.";
      }
    });
    _aiController.clear();
  }

  void _testSmartAlarm() {
    int inputAnswer = 0;
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) => AlertDialog(
        title: const Text('⏰ تحدي المنبه الذكي', textAlign: TextAlign.center),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            const Text('قم بحل هذه المسألة الرياضية لإغلاق المنبه:', textAlign: TextAlign.center),
            const SizedBox(height: 15),
            const Text('25 + 14 = ؟', style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold, color: Color(0xFF9D8BFF))),
            TextField(
              keyboardType: TextInputType.number,
              textAlign: TextAlign.center,
              onChanged: (v) => inputAnswer = int.tryParse(v) ?? 0,
            )
          ],
        ),
        actions: [
          TextButton(
            onPressed: () {
              if (inputAnswer == 39) {
                Navigator.pop(context);
                ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('إجابة صحيحة! تم إيقاف المنبه 🏆')));
              } else {
                HapticFeedback.vibrate();
              }
            },
            child: const Text('تأكيد وحل اللغز'),
          )
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('مساعد AI والمنبه الذكي'), backgroundColor: const Color(0xFF161926), centerTitle: true),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Card(
              color: Theme.of(context).cardColor,
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  children: [
                    const Icon(Icons.psychology, size: 50, color: Color(0xFF9D8BFF)),
                    const SizedBox(height: 10),
                    Text(_aiResponse, style: const TextStyle(fontSize: 15, height: 1.4), textAlign: TextAlign.center),
                    const SizedBox(height: 15),
                    TextField(
                      controller: _aiController,
                      textAlign: TextAlign.right,
                      decoratio
