<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>اختيار سورة اليوم</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: white;
        text-align: center;
        padding: 20px;
    }
    button {
        padding: 10px 20px;
        font-size: 18px;
        border: none;
        cursor: pointer;
        border-radius: 5px;
        margin: 5px;
    }
    #chooseBtn { background-color: green; color: white; }
    #readBtn { background-color: blue; color: white; }
    #audioBtn { background-color: orange; color: white; }
    #chosenSurah {
        font-size: 20px;
        margin-top: 20px;
        font-weight: bold;
    }
    #history {
        margin-top: 30px;
        border-top: 1px solid #ccc;
        padding-top: 10px;
    }
</style>
</head>
<body>

<h1>🌙 اختيار سورة اليوم</h1>
<p>اضغط على الزر لاختيار سورة عشوائية من القرآن الكريم</p>

<button id="chooseBtn">اختيار سورة</button>
<button id="readBtn" style="display:none;">عرض السورة</button>
<button id="audioBtn" style="display:none;">تشغيل الصوت</button>

<div id="chosenSurah"></div>

<div id="history">
    <h3>📜 السور التي قرأتها:</h3>
    <ul id="surahList"></ul>
</div>

<script>
    const surahs = [
        "الفاتحة", "البقرة", "آل عمران", "النساء", "المائدة", "الأنعام", "الأعراف", "الأنفال", "التوبة", "يونس", "هود", "يوسف",
        "الرعد", "إبراهيم", "الحجر", "النحل", "الإسراء", "الكهف", "مريم", "طه", "الأنبياء", "الحج", "المؤمنون", "النور",
        "الفرقان", "الشعراء", "النمل", "القصص", "العنكبوت", "الروم", "لقمان", "السجدة", "الأحزاب", "سبأ", "فاطر", "يس",
        "الصافات", "ص", "الزمر", "غافر", "فصلت", "الشورى", "الزخرف", "الدخان", "الجاثية", "الأحقاف", "محمد", "الفتح",
        "الحجرات", "ق", "الذاريات", "الطور", "النجم", "القمر", "الرحمن", "الواقعة", "الحديد", "المجادلة", "الحشر",
        "الممتحنة", "الصف", "الجمعة", "المنافقون", "التغابن", "الطلاق", "التحريم", "الملك", "القلم", "الحاقة", "المعارج",
        "نوح", "الجن", "المزمل", "المدثر", "القيامة", "الإنسان", "المرسلات", "النبأ", "النازعات", "عبس", "التكوير",
        "الإنفطار", "المطففين", "الإنشقاق", "البروج", "الطارق", "الأعلى", "الغاشية", "الفجر", "البلد", "الشمس", "الليل",
        "الضحى", "الشرح", "التين", "العلق", "القدر", "البينة", "الزلزلة", "العاديات", "القارعة", "التكاثر", "العصر",
        "الهمزة", "الفيل", "قريش", "الماعون", "الكوثر", "الكافرون", "النصر", "المسد", "الإخلاص", "الفلق", "الناس"
    ];

    let readSurahs = JSON.parse(localStorage.getItem("readSurahs")) || [];

    function updateHistory() {
        document.getElementById("surahList").innerHTML = readSurahs.map(s => `<li>${s}</li>`).join("");
    }

    function chooseSurah() {
        if (readSurahs.length === surahs.length) {
            alert("✅ لقد قرأت كل السور! سيتم إعادة التهيئة.");
            readSurahs = [];
        }
        let availableSurahs = surahs.filter(s => !readSurahs.includes(s));
        let randomSurah = availableSurahs[Math.floor(Math.random() * availableSurahs.length)];

        document.getElementById("chosenSurah").textContent = `📖 سورة اليوم: ${randomSurah}`;
        document.getElementById("readBtn").style.display = "inline-block";
        document.getElementById("audioBtn").style.display = "inline-block";

        if (!readSurahs.includes(randomSurah)) {
            readSurahs.push(randomSurah);
            localStorage.setItem("readSurahs", JSON.stringify(readSurahs));
        }

        updateHistory();

        document.getElementById("readBtn").onclick = () => {
            window.open(`https://quran.com/${surahs.indexOf(randomSurah) + 1}`, "_blank");
        };

        document.getElementById("audioBtn").onclick = () => {
            let audio = new Audio(`https://download.quranicaudio.com/qdc/mishari_al_afasy/murattal/${String(surahs.indexOf(randomSurah) + 1).padStart(3, '0')}.mp3`);
            audio.play();
        };
    }

    document.getElementById("chooseBtn").addEventListener("click", chooseSurah);

    updateHistory();
</script>

</body>
</html>
