[Uploading cdd (1).html…]()
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Thử Thách Du Lịch Việt Nam</title>
    <style>
        :root {
            --primary: #2563eb;
            --success: #22c55e;
            --error: #ef4444;
            --card-bg: rgba(255, 255, 255, 0.95);
            --text-main: #1e293b;
        }

        * { box-sizing: border-box; }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            margin: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            background-color: #333;
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            transition: background-image 0.8s ease-in-out;
            position: relative;
        }

        /* Lớp phủ để làm nổi bật khung câu hỏi */
        body::before {
            content: "";
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            z-index: 0;
        }

        .quiz-container {
            position: relative;
            z-index: 1;
            background: var(--card-bg);
            width: 100%;
            max-width: 650px;
            padding: 2rem;
            border-radius: 1.5rem;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        .header { text-align: center; margin-bottom: 1.5rem; }
        
        .cat-badge {
            display: inline-block;
            padding: 6px 16px;
            background: var(--primary);
            color: white;
            border-radius: 50px;
            font-size: 0.85rem;
            font-weight: bold;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .progress-container {
            height: 8px;
            background: #e2e8f0;
            border-radius: 4px;
            margin: 10px 0;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: var(--primary);
            width: 0%;
            transition: width 0.4s ease;
        }

        h2 { font-size: 1.3rem; line-height: 1.5; color: var(--text-main); margin-bottom: 1.5rem; }

        .options { display: flex; flex-direction: column; gap: 12px; }

        .option {
            padding: 1rem;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s;
            font-weight: 500;
            background: white;
        }

        .option:hover:not(.disabled) { border-color: var(--primary); background: #f0f7ff; }
        .option.correct { background: #dcfce7 !important; border-color: var(--success) !important; color: #166534; }
        .option.wrong { background: #fee2e2 !important; border-color: var(--error) !important; color: #991b1b; }
        .option.disabled { cursor: not-allowed; }

        .explanation {
            margin-top: 20px;
            padding: 15px;
            background: #f8fafc;
            border-left: 5px solid var(--primary);
            border-radius: 4px;
            display: none;
            font-style: italic;
            color: #475569;
            line-height: 1.5;
        }

        .btn {
            width: 100%;
            padding: 1rem;
            margin-top: 20px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            display: none;
            transition: opacity 0.2s;
        }

        .btn:hover { opacity: 0.9; }

        #result-screen { text-align: center; display: none; }
        .score-box { font-size: 3.5rem; font-weight: 800; color: var(--primary); margin: 20px 0; }
    </style>
</head>
<body>

<div class="quiz-container">
    <div id="quiz-ui">
        <div class="header">
            <div id="cat-badge" class="cat-badge">Đang tải...</div>
            <div class="progress-container"><div id="progress" class="progress-fill"></div></div>
            <small id="q-counter">Câu 1/20</small>
        </div>

        <h2 id="question-text">Câu hỏi sẽ hiển thị tại đây?</h2>
        
        <div id="options-container" class="options"></div>
        
        <div id="explanation-box" class="explanation"></div>
        
        <button id="next-btn" class="btn" onclick="nextQuestion()">Câu Tiếp Theo</button>
    </div>

    <div id="result-screen">
        <h1 style="margin-bottom: 0;">Chúc Mừng!</h1>
        <p>Bạn đã hoàn thành bài kiểm tra kiến thức du lịch.</p>
        <div class="score-box" id="final-score">0/20</div>
        <p id="feedback-text" style="font-weight: 500; margin-bottom: 30px;"></p>
        <button class="btn" style="display:block" onclick="location.reload()">Thử Thách Lại</button>
    </div>
</div>

<script>
    const quizData = [
        // --- TÀ XÙA (Câu 1-7) ---
        {
            cat: "TÀ XÙA",
            q: "Tà Xùa nằm ở độ cao trung bình khoảng bao nhiêu so với mực nước biển?",
            options: ["500m - 1.000m", "1.000m - 1.200m", "1.500m - 1.700m", "2.000m - 2.500m"],
            correct: 2,
            bg: "https://images.unsplash.com/photo-1583002821011-39686419574f?q=80&w=1600&auto=format&fit=crop",
            exp: "Tà Xùa nằm ở độ cao 1.500m - 1.700m, với những đỉnh cao hơn 2.800m."
        },
        {
            cat: "TÀ XÙA",
            q: "Đặc sản 'Sống lưng khủng long' tại Tà Xùa có đặc điểm gì?",
            options: ["Một hang động sâu", "Con đường mòn hẹp trên sống núi, hai bên là vực", "Một thác nước cao", "Cánh đồng hoa cải"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1596395819057-e37f55a8516b?q=80&w=1600&auto=format&fit=crop",
            exp: "Đây là con đường mòn hẹp nằm trên sống núi cao, mang lại tầm nhìn ngoạn mục ra biển mây."
        },
        {
            cat: "TÀ XÙA",
            q: "Loại cây nào là biểu tượng cho sự kiên cường và là nơi ngắm hoàng hôn đẹp ở Tà Xùa?",
            options: ["Cây thông ba lá", "Cây táo mèo cô đơn", "Cây chè Shan Tuyết", "Cây hoa ban"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1500382017468-9049fed747ef?q=80&w=1600&auto=format&fit=crop",
            exp: "Cây táo mèo cô đơn đứng lẻ loi trên đỉnh đồi là biểu tượng quen thuộc tại Tà Xùa."
        },
        {
            cat: "TÀ XÙA",
            q: "Thời điểm lý tưởng nhất để 'săn mây' Tà Xùa là khi nào?",
            options: ["Tháng 5 đến tháng 8", "Tháng 10 đến tháng 4 năm sau", "Tháng 6 đến tháng 9", "Quanh năm"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?q=80&w=1600&auto=format&fit=crop",
            exp: "Tháng 10 đến tháng 4 là mùa săn mây đẹp nhất do khí hậu lạnh khô và độ ẩm cao."
        },
        {
            cat: "TÀ XÙA",
            q: "Món cháo đặc sản nào của đồng bào Mông có vị đắng, ngọt, cay the?",
            options: ["Cháo ấu tẩu", "Cháo mắc nhung", "Cháo lươn", "Cháo ngô"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1547592166-23ac45744acd?q=80&w=1600&auto=format&fit=crop",
            exp: "Cháo mắc nhung là đặc sản độc đáo, rất thích hợp với tiết trời se lạnh của vùng cao."
        },
        {
            cat: "TÀ XÙA",
            q: "Khoảng cách từ Hà Nội đến Tà Xùa là bao nhiêu và mất bao lâu di chuyển?",
            options: ["100km - 3 tiếng", "200-240km - 5-6 tiếng", "350km - 8 tiếng", "500km - 10 tiếng"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1473679408190-0693dd22fe6a?q=80&w=1600&auto=format&fit=crop",
            exp: "Quãng đường khoảng 200-240km, mất 5-6 tiếng di chuyển qua các cung đường đèo dốc."
        },
        {
            cat: "TÀ XÙA",
            q: "Đồ uống nào là đặc sản nổi tiếng của vùng Tây Bắc thường thấy ở Tà Xùa?",
            options: ["Rượu cần", "Rượu táo mèo", "Rượu ngô", "Nước dừa"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1510626176961-4b57d4fbad03?q=80&w=1600&auto=format&fit=crop",
            exp: "Rượu táo mèo có vị thơm nồng, ấm bụng, là đặc trưng của vùng này."
        },

        // --- HỘI AN (Câu 8-14) ---
        {
            cat: "HỘI AN",
            q: "UNESCO công nhận Phố cổ Hội An là Di sản Văn hóa Thế giới vào năm nào?",
            options: ["1990", "1995", "1999", "2005"],
            correct: 2,
            bg: "https://images.unsplash.com/photo-1555932845-80227917730e?q=80&w=1600&auto=format&fit=crop",
            exp: "Phố cổ Hội An được UNESCO vinh danh vào năm 1999."
        },
        {
            cat: "HỘI AN",
            q: "Biểu tượng kiến trúc nổi tiếng nhất của Hội An, kết hợp văn hóa Việt - Nhật là gì?",
            options: ["Nhà cổ Tấn Ký", "Hội quán Phúc Kiến", "Chùa Cầu", "Nhà thờ tộc Trần"],
            correct: 2,
            bg: "https://images.unsplash.com/photo-1599708153386-62e242485567?q=80&w=1600&auto=format&fit=crop",
            exp: "Chùa Cầu (Cầu Nhật Bản) là biểu tượng lịch sử và văn hóa độc đáo nhất của Hội An."
        },
        {
            cat: "HỘI AN",
            q: "Kiến trúc nhà ở đặc trưng trong phố cổ Hội An thường có hình dạng gì?",
            options: ["Nhà sàn", "Nhà hình ống", "Nhà chữ U", "Nhà biệt thự"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1590508682121-1773998b6b23?q=80&w=1600&auto=format&fit=crop",
            exp: "Nhà cổ Hội An chủ yếu là nhà hình ống, tường vàng, mái ngói rêu phong."
        },
        {
            cat: "HỘI AN",
            q: "Món mì đặc trưng nhất của Hội An với sợi mì dai, thịt xá xíu và nước sốt đậm đà là?",
            options: ["Hủ tiếu", "Mì Quảng", "Cao lầu", "Bún bò"],
            correct: 2,
            bg: "https://images.unsplash.com/photo-1504674900247-0877df9cc836?q=80&w=1600&auto=format&fit=crop",
            exp: "Cao lầu là linh hồn ẩm thực Hội An, món ăn không thể trộn lẫn với bất kỳ nơi nào khác."
        },
        {
            cat: "HỘI AN",
            q: "Hoạt động lãng mạn nào thường diễn ra trên sông Hoài vào buổi tối?",
            options: ["Đua thuyền công", "Thả hoa đăng", "Câu cá đêm", "Múa lân dưới nước"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1621350614833-28989528f804?q=80&w=1600&auto=format&fit=crop",
            exp: "Thả đèn hoa đăng trên sông Hoài tạo nên không gian lung linh, huyền ảo cho phố cổ."
        },
        {
            cat: "HỘI AN",
            q: "Thời điểm lý tưởng nhất để tham quan Hội An trong năm là?",
            options: ["Tháng 2 đến tháng 4", "Tháng 10 đến tháng 12", "Tháng 6 đến tháng 8", "Tháng 1"],
            correct: 0,
            bg: "https://images.unsplash.com/photo-1588661706689-d10c81498b53?q=80&w=1600&auto=format&fit=crop",
            exp: "Tháng 2-4 là lúc thời tiết xuân mát mẻ, ít mưa, nắng nhẹ, rất đẹp để khám phá."
        },
        {
            cat: "HỘI AN",
            q: "Để di chuyển đến Hội An từ sân bay gần nhất, du khách sẽ hạ cánh tại đâu?",
            options: ["Sân bay Phú Bài", "Sân bay Chu Lai", "Sân bay Đà Nẵng", "Sân bay Cam Ranh"],
            correct: 2,
            bg: "https://images.unsplash.com/photo-1527631746610-bca00a040d60?q=80&w=1600&auto=format&fit=crop",
            exp: "Hội An cách sân bay Đà Nẵng khoảng 30km, rất thuận tiện di chuyển."
        },

        // --- VĨNH HY (Câu 15-20) ---
        {
            cat: "VỊNH VĨNH HY",
            q: "Vịnh Vĩnh Hy nằm ở huyện nào của tỉnh Ninh Thuận?",
            options: ["Huyện Ninh Phước", "Huyện Ninh Hải", "Huyện Thuận Bắc", "Huyện Ninh Sơn"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1506461883276-594a12b11cf3?q=80&w=1600&auto=format&fit=crop",
            exp: "Vịnh Vĩnh Hy thuộc xã Vĩnh Hải, huyện Ninh Hải."
        },
        {
            cat: "VỊNH VĨNH HY",
            q: "Điểm đến nào gần Vĩnh Hy có những bãi đá cổ xếp chồng tạo hiệu ứng 'thác nước trên biển'?",
            options: ["Bãi Kinh", "Hang Rái", "Hòn Đỏ", "Mũi Dinh"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1537210249814-b9a10a161ae4?q=80&w=1600&auto=format&fit=crop",
            exp: "Hang Rái nổi tiếng với cảnh quan kỳ vĩ, đặc biệt khi sóng vỗ mạnh tạo thác nước trên biển."
        },
        {
            cat: "VỊNH VĨNH HY",
            q: "Hệ sinh thái đặc biệt nào sát cạnh Vĩnh Hy được UNESCO công nhận là khu bảo tồn sinh quyển thế giới?",
            options: ["Vườn Quốc gia Cúc Phương", "Vườn Quốc gia Núi Chúa", "Rừng ngập mặn Cần Giờ", "Vườn Quốc gia Phong Nha"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1502082553048-f009c37129b9?q=80&w=1600&auto=format&fit=crop",
            exp: "Núi Chúa có hệ sinh thái bán khô hạn độc đáo hòa quyện giữa núi rừng và biển."
        },
        {
            cat: "VỊNH VĨNH HY",
            q: "Món gỏi đặc sản tươi mát nào được làm từ cá tươi đánh bắt tại vịnh?",
            options: ["Gỏi cá trích", "Gỏi cá mai", "Gỏi sứa", "Gỏi cá hồi"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1546069901-ba9599a7e63c?q=80&w=1600&auto=format&fit=crop",
            exp: "Gỏi cá mai ăn kèm rau thơm, lạc rang và nước chấm chua ngọt là món ăn không thể bỏ qua."
        },
        {
            cat: "VỊNH VĨNH HY",
            q: "Thời điểm lý tưởng nhất để lặn ngắm san hô và tắm biển Vĩnh Hy là?",
            options: ["Tháng 9 đến tháng 11", "Tháng 5 đến tháng 8", "Tháng 12 đến tháng 2", "Quanh năm"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1544551763-46a013bb70d5?q=80&w=1600&auto=format&fit=crop",
            exp: "Tháng 5-8 là lúc biển êm, nước trong xanh ngọc bích, lý tưởng nhất cho hoạt động dưới nước."
        },
        {
            cat: "VỊNH VĨNH HY",
            q: "Sản phẩm đặc sản nào của vùng đất Ninh Thuận du khách có thể mua tại vườn gần Vĩnh Hy?",
            options: ["Dừa xiêm", "Nho và các sản phẩm từ nho", "Thanh long", "Bưởi da xanh"],
            correct: 1,
            bg: "https://images.unsplash.com/photo-1533470192478-9997bef539ec?q=80&w=1600&auto=format&fit=crop",
            exp: "Ninh Thuận là vựa nho lớn nhất Việt Nam, nổi tiếng với rượu nho và mật nho."
        }
    ];

    let currentIdx = 0;
    let score = 0;
    let answered = false;

    function initQuiz() {
        const data = quizData[currentIdx];
        
        // Cập nhật giao diện
        document.body.style.backgroundImage = `url('${data.bg}')`;
        document.getElementById('cat-badge').innerText = data.cat;
        document.getElementById('question-text').innerText = data.q;
        document.getElementById('q-counter').innerText = `Câu ${currentIdx + 1}/${quizData.length}`;
        document.getElementById('progress').style.width = `${((currentIdx) / quizData.length) * 100}%`;
        
        // Reset trạng thái
        const container = document.getElementById('options-container');
        container.innerHTML = '';
        document.getElementById('explanation-box').style.display = 'none';
        document.getElementById('next-btn').style.display = 'none';
        answered = false;

        // Tạo options
        data.options.forEach((opt, i) => {
            const div = document.createElement('div');
            div.className = 'option';
            div.innerText = opt;
            div.onclick = () => checkAnswer(i, div);
            container.appendChild(div);
        });
    }

    function checkAnswer(idx, el) {
        if (answered) return;
        answered = true;
        
        const correct = quizData[currentIdx].correct;
        const options = document.querySelectorAll('.option');

        if (idx === correct) {
            el.classList.add('correct');
            score++;
        } else {
            el.classList.add('wrong');
            options[correct].classList.add('correct');
        }

        // Vô hiệu hóa click
        options.forEach(opt => opt.classList.add('disabled'));
        
        // Hiển thị giải thích & nút tiếp theo
        const expBox = document.getElementById('explanation-box');
        expBox.innerHTML = `<strong>Giải thích:</strong> ${quizData[currentIdx].exp}`;
        expBox.style.display = 'block';
        document.getElementById('next-btn').style.display = 'block';
    }

    function nextQuestion() {
        currentIdx++;
        if (currentIdx < quizData.length) {
            initQuiz();
        } else {
            showResult();
        }
    }

    function showResult() {
        document.getElementById('quiz-ui').style.display = 'none';
        const res = document.getElementById('result-screen');
        res.style.display = 'block';
        document.getElementById('final-score').innerText = `${score}/${quizData.length}`;
        document.getElementById('progress').style.width = '100%';
        
        let feedback = "";
        const ratio = score / quizData.length;
        if (ratio === 1) feedback = "Xuất sắc! Bạn là chuyên gia du lịch thực thụ.";
        else if (ratio >= 0.7) feedback = "Rất tốt! Bạn nắm kiến thức rất chắc.";
        else if (ratio >= 0.5) feedback = "Khá ổn. Hãy tìm hiểu thêm để có chuyến đi tuyệt vời nhé!";
        else feedback = "Cố gắng lên! Đọc kỹ bài giới thiệu và thử lại nhé.";
        
        document.getElementById('feedback-text').innerText = feedback;
        document.body.style.backgroundImage = "url('https://images.unsplash.com/photo-1476514525535-07fb3b4ae5f1?q=80&w=1600&auto=format&fit=crop')";
    }

    // Bắt đầu
    initQuiz();
</script>

</body>
</html>
