
<html lang="vi">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Học Tiếng Anh</title>
    <style>
        body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color:
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        margin: 0;
    }

    .container {
        background: white;
        padding: 30px;
        border-radius: 15px;
        box-shadow: 0 5px 20px rgba(0, 0, 0, 0.1);
        text-align: center;
        width: 90%;
        max-width: 600px;
    }

    #score {
        font-weight: 600;
        color: #007bff;
        padding: 10px;
        margin-bottom: 20px;
        border-bottom: 1px solid #eee;
    }

    select {
        width: calc(100% - 30px);
        padding: 12px;
        margin-bottom: 20px;
        border: 1px solid #ddd;
        border-radius: 8px;
        appearance: none;
        background-image: linear-gradient(45deg, transparent 50%, gray 50%), linear-gradient(135deg, gray 50%, transparent 50%);
        background-position: calc(100% - 15px) calc(1em + 2px), calc(100% - 10px) calc(1em + 2px);
        background-size: 5px 5px, 5px 5px;
        background-repeat: no-repeat;
    }

    .header-answer, .body-answer {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 20px;
    }

    .answer-input {
        width: 60%;
        display: flex;
        align-items: center;
    }

    .answer-input input {
        width: calc(100% - 40px);
        padding: 12px;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-left: 10px;
    }

    .pointer {
        cursor: pointer;
        font-size: 1.2em;
        color: #007bff;
    }

    .button-row {
        display: flex;
        justify-content: space-between;
        width: 38%;
    }

    button {
        padding: 12px 20px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        background-color: #007bff;
        color: white;
        font-weight: 600;
        transition: background-color 0.3s ease;
    }

    button:hover {
        background-color: #0056b3;
    }

    #message {
        color: red;
        margin-top: 10px;
        font-weight: 600;
    }

    #english {
        color: #28a745;
        font-weight: 600;
        margin-top: 10px;
    }

    #vietnamese {
        color: #6c757d;
        margin-top: 5px;
    }

    .row {
        display: flex;
        justify-content: space-between;
        margin-top: 20px;
    }

    /* Responsive Design */
    @media (max-width: 600px) {
        .container {
            width: 95%;
            padding: 20px;
        }

        .body-answer {
            flex-direction: column;
        }

        .answer-input {
            width: 100%;
            margin-bottom: 10px;
        }

        .button-row {
            width: 100%;
            margin-top: 10px;
        }

        button {
            width: 48%;
        }

        .row {
            flex-direction: column;
            align-items: center;
        }

        .row button {
            width: 100%;
            margin-top: 10px;
        }
    }
    </style>
</head>

<body>
    <div class="container">
        <div class="header-answer">
            <div id="score">Điểm: 0/0</div>
            <select id="voiceSelect"></select>
        </div>
        <div class="body-answer">
            <div class="answer-input">
                <span onclick="speak()" class="pointer">🔊</span>
                <input type="text" id="answer" style="width: 80%;" placeholder="Enter the answer..."
                    autocomplete="off">
            </div>
            <div class="button-row">
                <button onclick="checkAnswer()">✔ Check </button>
                <button onclick="showAnswer()">👀 Answer</button>
            </div>
        </div>
        <p id="message"></p>
        <p id="english" style="color: green; font-weight: bold;"></p>
        <p id="vietnamese" class="text-gray"></p>
        <div class="row">
            <button onclick="prevSentence()">⬅ pre</button>
            <button onclick="nextSentence()">➡ Next</button>
        </div>
    </div>

    <script>
        const sentences = [
            { en: "The weather is nice today.", vi: "Thời tiết hôm nay đẹp." },
            { en: "I love learning new languages.", vi: "Tôi thích học ngôn ngữ mới." },
            { en: "Practice makes perfect.", vi: "Luyện tập làm nên hoàn hảo." },
            { en: "She is reading a book.", vi: "Cô ấy đang đọc sách." },
            { en: "They are playing football.", vi: "Họ đang chơi bóng đá." },
            { en: "I have a meeting at 3 PM.", vi: "Tôi có một cuộc họp lúc 3 giờ chiều." },
            { en: "Can you help me, please?", vi: "Bạn có thể giúp tôi không?" },
            { en: "Where is the nearest bus stop?", vi: "Trạm xe buýt gần nhất ở đâu?" },
            { en: "I need to buy some groceries.", vi: "Tôi cần mua một số thực phẩm." },
            { en: "This is my favorite song.", vi: "Đây là bài hát yêu thích của tôi." },
            { en: "Let's go to the park.", vi: "Hãy đi đến công viên." },
            { en: "What time does the train arrive?", vi: "Chuyến tàu đến lúc mấy giờ?" },
            { en: "She speaks English fluently.", vi: "Cô ấy nói tiếng Anh trôi chảy." },
            { en: "I will call you later.", vi: "Tôi sẽ gọi cho bạn sau." },
            { en: "My father is a doctor.", vi: "Bố tôi là bác sĩ." },
            { en: "This coffee is too hot.", vi: "Cà phê này quá nóng." },
            { en: "We need more chairs.", vi: "Chúng tôi cần thêm ghế." },
            { en: "He drives a red car.", vi: "Anh ấy lái một chiếc xe màu đỏ." },
            { en: "It's raining outside.", vi: "Trời đang mưa bên ngoài." },
            { en: "She has long hair.", vi: "Cô ấy có mái tóc dài." },
            { en: "I forgot my keys.", vi: "Tôi quên chìa khóa của mình." },
            { en: "Do you like spicy food?", vi: "Bạn có thích đồ ăn cay không?" },
            { en: "We are going on vacation next week.", vi: "Chúng tôi sẽ đi nghỉ vào tuần tới." },
            { en: "This dress looks beautiful.", vi: "Chiếc váy này trông đẹp quá." },
            { en: "I have never been to Japan.", vi: "Tôi chưa bao giờ đến Nhật Bản." },
            { en: "He is watching TV now.", vi: "Anh ấy đang xem TV bây giờ." },
            { en: "Let's meet at the cafe.", vi: "Hãy gặp nhau ở quán cà phê." },
            { en: "This is a difficult question.", vi: "Đây là một câu hỏi khó." },
            { en: "She loves painting.", vi: "Cô ấy thích vẽ tranh." },
            { en: "We should go home now.", vi: "Chúng ta nên về nhà ngay." },
            { en: "I need to wake up early tomorrow.", vi: "Tôi cần dậy sớm vào ngày mai." },
            { en: "She is good at playing the piano.", vi: "Cô ấy giỏi chơi đàn piano." },
            { en: "The movie was very interesting.", vi: "Bộ phim rất thú vị." },
            { en: "He likes to travel around the world.", vi: "Anh ấy thích đi du lịch khắp thế giới." },
            { en: "I will send you an email.", vi: "Tôi sẽ gửi cho bạn một email." },
            { en: "My house is near the river.", vi: "Nhà tôi gần con sông." },
            { en: "This cake is very delicious.", vi: "Chiếc bánh này rất ngon." },
            { en: "They are waiting for the bus.", vi: "Họ đang đợi xe buýt." },
            { en: "Can you speak more slowly?", vi: "Bạn có thể nói chậm hơn không?" },
            { en: "She always wakes up early.", vi: "Cô ấy luôn dậy sớm." },
            { en: "I need to charge my phone.", vi: "Tôi cần sạc điện thoại." },
            { en: "My sister is studying in the library.", vi: "Em gái tôi đang học ở thư viện." },
            { en: "We are having dinner now.", vi: "Chúng tôi đang ăn tối bây giờ." },
            { en: "He is very kind to everyone.", vi: "Anh ấy rất tốt với mọi người." },
            { en: "The restaurant is very crowded today.", vi: "Nhà hàng hôm nay rất đông." },
            { en: "She is writing an article.", vi: "Cô ấy đang viết một bài báo." },
            { en: "Let's take a break.", vi: "Hãy nghỉ một chút đi." },
            { en: "I am feeling tired.", vi: "Tôi cảm thấy mệt." },
            { en: "He works as a software engineer.", vi: "Anh ấy làm kỹ sư phần mềm." },
            { en: "This hotel has a swimming pool.", vi: "Khách sạn này có hồ bơi." },
            { en: "I need to buy a new laptop.", vi: "Tôi cần mua một chiếc laptop mới." },
            { en: "She is wearing a beautiful dress.", vi: "Cô ấy đang mặc một chiếc váy đẹp." },
            { en: "The book is on the table.", vi: "Cuốn sách ở trên bàn." },
            { en: "They are talking about the project.", vi: "Họ đang nói về dự án." },
            { en: "Do you have any brothers or sisters?", vi: "Bạn có anh chị em nào không?" },
            { en: "The cat is sleeping under the chair.", vi: "Con mèo đang ngủ dưới ghế." },
            { en: "I need some time to think.", vi: "Tôi cần chút thời gian để suy nghĩ." },
            { en: "We are planning a trip to the beach.", vi: "Chúng tôi đang lên kế hoạch đi biển." },
            { en: "She is afraid of heights.", vi: "Cô ấy sợ độ cao." },
            { en: "I enjoy reading books in my free time.", vi: "Tôi thích đọc sách vào thời gian rảnh." },
            { en: "The sun is shining brightly.", vi: "Mặt trời đang chiếu sáng rực rỡ." },
            { en: "My parents are very supportive.", vi: "Bố mẹ tôi rất ủng hộ tôi." },
            { en: "He is learning to cook.", vi: "Anh ấy đang học nấu ăn." },
            { en: "She likes listening to music.", vi: "Cô ấy thích nghe nhạc." },
            { en: "The bus is running late.", vi: "Xe buýt đang bị trễ." },
            { en: "We had a great time at the party.", vi: "Chúng tôi đã có khoảng thời gian tuyệt vời ở bữa tiệc." },
            { en: "She can swim very well.", vi: "Cô ấy bơi rất giỏi." },
            { en: "I need to drink some water.", vi: "Tôi cần uống một chút nước." },
            { en: "He is a very funny person.", vi: "Anh ấy là một người rất hài hước." },
            { en: "They are discussing an important topic.", vi: "Họ đang thảo luận một chủ đề quan trọng." },
            { en: "The train station is not far from here.", vi: "Nhà ga không xa lắm từ đây." },
            { en: "The sun rises in the east.", vi: "Mặt trời mọc ở hướng đông." },
            { en: "Water boils at 100 degrees Celsius.", vi: "Nước sôi ở 100 độ C." },
            { en: "Cats are independent animals.", vi: "Mèo là loài động vật độc lập." },
            { en: "Dogs are loyal companions.", vi: "Chó là những người bạn đồng hành trung thành." },
            { en: "I enjoy reading before bed.", vi: "Tôi thích đọc sách trước khi đi ngủ." },
            { en: "She loves to paint landscapes.", vi: "Cô ấy thích vẽ tranh phong cảnh." },
            { en: "He plays the guitar beautifully.", vi: "Anh ấy chơi đàn guitar rất hay." },
            { en: "We are learning about space.", vi: "Chúng tôi đang học về vũ trụ." },
            { en: "They visited the museum yesterday.", vi: "Họ đã đến thăm bảo tàng ngày hôm qua." },
            { en: "The flowers are blooming in the garden.", vi: "Những bông hoa đang nở trong vườn." },
            { en: "I like to drink coffee in the morning.", vi: "Tôi thích uống cà phê vào buổi sáng." },
            { en: "She prefers tea with milk.", vi: "Cô ấy thích trà với sữa." },
            { en: "He is studying for his exams.", vi: "Anh ấy đang ôn thi." },
            { en: "We are planning a picnic this weekend.", vi: "Chúng tôi đang lên kế hoạch cho một buổi dã ngoại cuối tuần này." },
            { en: "They are building a new house.", vi: "Họ đang xây một ngôi nhà mới." },
            { en: "The birds are singing in the trees.", vi: "Chim đang hót trên cây." },
            { en: "I need to buy a new phone.", vi: "Tôi cần mua một chiếc điện thoại mới." },
            { en: "She is cooking dinner for her family.", vi: "Cô ấy đang nấu bữa tối cho gia đình." },
            { en: "He is driving to work.", vi: "Anh ấy đang lái xe đi làm." },
            { en: "We are watching a movie tonight.", vi: "Chúng tôi sẽ xem phim tối nay." },
            { en: "They are traveling to Europe next month.", vi: "Họ sẽ đi du lịch châu Âu vào tháng tới." },
            { en: "The children are playing in the park.", vi: "Trẻ em đang chơi trong công viên." },
            { en: "I am going to the store.", vi: "Tôi đang đi đến cửa hàng." },
            { en: "She is taking a walk.", vi: "Cô ấy đang đi dạo." },
            { en: "He is listening to music.", vi: "Anh ấy đang nghe nhạc." },
            { en: "We are eating lunch.", vi: "Chúng tôi đang ăn trưa." },
            { en: "They are studying English.", vi: "Họ đang học tiếng Anh." },
            { en: "The car is parked in the garage.", vi: "Xe ô tô đang đỗ trong gara." },
            { en: "I am happy to see you.", vi: "Tôi rất vui khi gặp bạn." },
        ];


        let index = 0;
        let speechInstance = null;
        let speakTimeout;
        let score = 0;
        let showedAnswer = [];

        function updateSentence() {
            document.getElementById("answer").value = "";
            document.getElementById("message").textContent = "";
            document.getElementById("vietnamese").style.display = "none";
            document.getElementById("english").style.display = "none";
        }

        function populateVoices() {
            const voiceSelect = document.getElementById("voiceSelect");
            const voices = speechSynthesis.getVoices().filter(voice => voice.lang.startsWith("en"));

            voiceSelect.innerHTML = voices.map(voice => `<option value="${voice.name}">${voice.name}</option>`).join("");

            const defaultVoice = voices.find(voice => voice.name === "Google UK English Male") || voices[0];
            if (defaultVoice) {
                voiceSelect.value = defaultVoice.name;
            }
        }

        function speak(repeat = 1, delay = 3000) {
            if (speechInstance) {
                speechSynthesis.cancel();
                clearTimeout(speakTimeout);
            }

            speechInstance = new SpeechSynthesisUtterance(sentences[index].en);
            speechInstance.lang = "en-GB";
            speechInstance.rate = 1;
            const selectedVoice = document.getElementById("voiceSelect").value;
            speechInstance.voice = speechSynthesis.getVoices().find(voice => voice.name === selectedVoice);

            let count = 0;
            function speakAgain() {
                if (count < repeat) {
                    speechSynthesis.speak(speechInstance);
                    count++;
                    speakTimeout = setTimeout(() => {
                        if (count < repeat) speakAgain();
                    }, delay);
                }
            }
            speakAgain();
        }

        function checkAnswer() {
            const userAnswer = document.getElementById("answer").value.trim().replace(/[.,?!]/g, "");
            const correctAnswer = sentences[index].en.replace(/[.,?!]/g, "");

            if (userAnswer.toLowerCase() === correctAnswer.toLowerCase()) {
                console.log("showedAnswer111: ", showedAnswer)
                console.log("showedAnswer111: ", index)

                if (!showedAnswer.includes(index)) {
                    score++;
                    console.log("da vao: ", score)
                    showedAnswer.push(index);
                }
                document.getElementById("message").textContent = "✅ Chính xác!";
                document.getElementById("message").style.color = "green";
                document.getElementById("vietnamese").textContent = sentences[index].vi;
                document.getElementById("vietnamese").style.display = "block";
                document.getElementById("english").textContent = sentences[index].en;
                document.getElementById("english").style.display = "block";
                updateScore();
            } else {
                document.getElementById("message").textContent = "❌ Sai rồi! Hãy thử lại.";
                document.getElementById("message").style.color = "red";
            }
        }

        function nextSentence() {
            speechSynthesis.cancel();
            clearTimeout(speakTimeout);
            index = (index + 1) % sentences.length;
            updateSentence();
            speak(3);
        }

        function prevSentence() {
            speechSynthesis.cancel();
            clearTimeout(speakTimeout);
            index = (index - 1 + sentences.length) % sentences.length;
            updateSentence();
            speak(3);
        }

        document.addEventListener("keydown", function (event) {
            if (event.altKey && event.key === "ArrowRight") {
                nextSentence();
            } else if (event.altKey && event.key === "ArrowLeft") {
                prevSentence();
            } else if (event.key === "Enter") {
                checkAnswer();
            }
        });

        function showAnswer() {
            document.getElementById("english").textContent = sentences[index].en;
            document.getElementById("english").style.display = "block";
            if (!showedAnswer.includes(index)) {
                showedAnswer.push(index);
            }
            console.log("showedAnswer: ", showedAnswer)
        }

        function updateScore() {
            document.getElementById("score").textContent = `Điểm: ${score}/${sentences.length}`;
        }

        speechSynthesis.onvoiceschanged = populateVoices;

        window.onload = () => {
            populateVoices();
            setTimeout(() => {
                speak(1);
            }, 500);
            updateScore();
        };
    </script>
</body>

</html>
