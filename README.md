<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>教育热点文章生成器</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f9f9f9; }
    h1 { color: #2c3e50; }
    textarea, input, button { font-size: 16px; margin-top: 10px; width: 100%; }
    #output { white-space: pre-wrap; background: #fff; padding: 10px; border: 1px solid #ddd; margin-top: 20px; }
  </style>
</head>
<body>
  <h1>教育热点文章生成器</h1>
  <label for="score">请输入分数：</label>
  <input type="number" id="score" value="600">
  <div class="controls">
    <button onclick="generateArticle()">生成文章</button>
    <button onclick="copyToClipboard()">一键复制</button>
  </div>

  <div id="output"></div>

  <script>
    const newsMock = [
      { title: "教育部发布高考改革新政", url: "https://www.moe.gov.cn/news1.html" },
      { title: "某地中考纳入实验操作", url: "https://www.moe.gov.cn/news2.html" }
    ];

    const schoolData = [
      { name: '复旦大学', min_score: 650, region: '华东', type: '985' },
      { name: '华中科技大学', min_score: 615, region: '华中', type: '双一流' },
      { name: 'XX学院', min_score: 530, region: '华南', type: '民办' }
    ];

    function recommendSchools(score) {
      return schoolData.filter(s => s.min_score <= score);
    }

    function generatePoll() {
      const sample = [...schoolData].sort(() => Math.random() - 0.5).slice(0, 2);
      return `你更喜欢哪所学校？<br>A. ${sample[0].name}<br>B. ${sample[1].name}`;
    }

    function generateArticle() {
      const score = parseInt(document.getElementById('score').value);
      const schools = recommendSchools(score);
      const poll = generatePoll();

      let html = `<strong>【今日教育快讯 - ${new Date().toISOString().slice(0, 10)}】</strong><br><br>`;
      html += `📢 教育热点：<br>`;
      html += newsMock.map(n => `<a href='${n.url}' target='_blank'><strong>${n.title}</strong></a>`).join("<br>") + "<br>";

      html += `🎯 学校推荐（按您的成绩）：<br>`;
      html += schools.map(s => `- <strong>${s.name}</strong>（${s.type}，最低录取分：${s.min_score}）`).join("<br>") + "<br><br>";

      html += `🗳️ 互动话题：<br>${poll}<br><br>`;
      html += `<em>本内容由AI助手自动生成，数据来源教育公开平台。</em>`;

      document.getElementById("output").innerHTML = html;
    }

    function copyToClipboard() {
      const container = document.createElement("textarea");
      container.innerHTML = document.getElementById("output").innerHTML.replace(/<br>/g, "\\n").replace(/<[^>]*>/g, ''); // 文本纯净化
      document.body.appendChild(container);
      container.select();
      document.execCommand("copy");
      document.body.removeChild(container);
      alert("文章已复制，可粘贴到公众号编辑器中！");
    }
  </script>
</body>
</html>

