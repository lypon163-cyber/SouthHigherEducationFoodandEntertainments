<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>南高教园区新生美食雷达 & 避雷针</title>
    <style>
        :root {
            --primary-color: #ff6b6b;
            --secondary-color: #ff8e53;
            --bg-color: #f7f9fc;
            --text-dark: #2c3e50;
            --danger-color: #fa5252;
            --success-color: #40c057;
        }

        body {
            margin: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-dark);
            padding-bottom: 60px;
        }

        /* 潮流多巴胺渐变车头 */
        header {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            padding: 30px 20px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
        }

        header h1 {
            margin: 0;
            font-size: 26px;
            letter-spacing: 1px;
        }

        header p {
            margin: 8px 0 0 0;
            opacity: 0.9;
            font-size: 14px;
        }

        .container {
            max-width: 700px;
            margin: 20px auto;
            padding: 0 15px;
        }

        /* 搜索与筛选区 */
        .search-box {
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            margin-bottom: 20px;
            display: flex;
            gap: 10px;
        }

        .search-box select, .search-box input {
            padding: 10px;
            border: 1px solid #eceff1;
            border-radius: 8px;
            outline: none;
            font-size: 14px;
        }

        .search-box select { width: 30%; }
        .search-box input { flex: 1; }

        /* 选项卡导航 */
        .tabs {
            display: flex;
            background: #eddcd2;
            background: rgba(0,0,0,0.05);
            padding: 4px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .tab-btn {
            flex: 1;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            border-radius: 6px;
            font-weight: bold;
            font-size: 14px;
            transition: all 0.2s;
        }

        .tab-btn.active {
            background: white;
            color: var(--primary-color);
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        /* 动态商铺/情报卡片 */
        .card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            margin-bottom: 15px;
            border-left: 5px solid #ddd;
            transition: transform 0.2s;
        }

        .card:hover { transform: translateY(-2px); }
        .card.red-list { border-left-color: var(--success-color); }
        .card.black-list { border-left-color: var(--danger-color); }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .store-name {
            font-size: 18px;
            font-weight: bold;
        }

        .badge {
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 12px;
            font-weight: bold;
        }

        .badge.red { background: #ffe3e3; color: var(--danger-color); }
        .badge.green { background: #e6fcf5; color: var(--success-color); }
        .badge.orange { background: #fff4e6; color: var(--secondary-color); }

        .meta-info {
            font-size: 13px;
            color: #78909c;
            margin-bottom: 10px;
        }

        .review-text {
            font-size: 14px;
            background: #f8f9fa;
            padding: 12px;
            border-radius: 8px;
            color: #546e7a;
            line-height: 1.5;
        }

        /* 发布表单弹窗风 */
        .publish-box {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
            margin-top: 25px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-size: 14px;
            font-weight: bold;
        }

        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #eceff1;
            border-radius: 8px;
            box-sizing: border-box;
        }

        .submit-btn {
            width: 100%;
            padding: 12px;
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            font-size: 15px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <header>
        <h1>🍊 南高教园区美食雷达</h1>
        <p>学府一号 / 钱湖天地 / 步行街 —— 专属新生的野生避雷与饭搭子互助指南</p>
    </header>

    <div class="container">
        <div class="search-box">
            <select id="areaFilter" onchange="renderData()">
                <option value="all">全部商圈</option>
                <option value="学府一号">学府一号</option>
                <option value="钱湖天地">钱湖天地</option>
                <option value="万里步行街">万里步行街</option>
            </select>
            <input type="text" id="searchInput" placeholder="输入店名搜索...（例：烤肉）" oninput="renderData()">
        </div>

        <div class="tabs">
            <div class="tab-btn active" id="tab-reviews" onclick="switchTab('reviews')">📢 真实红黑榜</div>
            <div class="tab-btn" id="tab-parties" onclick="switchTab('parties')">🍗 拼单饭搭子</div>
        </div>

        <div id="content-list"></div>

        <div class="publish-box">
            <h3 style="margin-top:0; border-bottom: 1px solid #eee; padding-bottom: 10px;" id="form-title">我要发帖排雷/推荐</h3>
            <div id="review-form-fields">
                <div class="form-group">
                    <label>店铺名称</label>
                    <input type="text" id="form-store-name" placeholder="请输入店铺全称">
                </div>
                <div class="form-group">
                    <label>所属商圈</label>
                    <select id="form-area">
                        <option value="学府一号">学府一号</option>
                        <option value="钱湖天地">钱湖天地</option>
                        <option value="万里步行街">万里步行街</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>评价性质</label>
                    <select id="form-type">
                        <option value="黑榜避雷">⚠️ 黑榜避雷（老生大实话）</option>
                        <option value="红榜推荐">👍 红榜推荐（宝藏店铺）</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>人均消费 (元)</label>
                    <input type="number" id="form-price" value="30">
                </div>
                <div class="form-group">
                    <label>大实话吐槽/推荐语</label>
                    <textarea id="form-content" rows="3" placeholder="吃了拉肚子？还是分量特别足？请留下真实评价..."></textarea>
                </div>
            </div>

            <div id="party-form-fields" style="display: none;">
                <div class="form-group">
                    <label>聚餐主题</label>
                    <input type="text" id="form-party-title" placeholder="例如：今晚钱湖天地海底捞拼满减缺2人">
                </div>
                <div class="form-group">
                    <label>目标人数</label>
                    <input type="number" id="form-party-slots" value="4">
                </div>
            </div>

            <button class="submit-btn" onclick="handleSubmit()">立即向全校发布</button>
        </div>
    </div>

    <script>
        // --- 核心模拟数据库 (前端内循环内存) ---
        let currentTab = 'reviews';
        
        let initialReviews = [
            { id: 1, name: "学府一号 XX 旋转小火锅", area: "学府一号", type: "黑榜避雷", price: 35, text: "大二学长排雷：丸子全是面粉，汤底咸得发苦。吃完回宿舍拉了三天，新生千万别去交学费！", date: "刚刚" },
            { id: 2, name: "钱湖天地 东北泥炉烤肉", area: "钱湖天地", type: "红榜推荐", price: 65, text: "万里大三学姐推荐：肉量超级扎实！凭学生证还送一份大拉皮，宿舍开学聚餐首选，绝对不踩雷。", date: "1小时前" },
            { id: 3, name: "万里步行街 无名炸串", area: "万里步行街", type: "黑榜避雷", price: 18, text: "实训周续命避雷：千万别点他们家的深夜外卖，用的油一股怪味，而且巨辣，第二天直接起不来床去实训！", date: "2小时前" }
        ];

        let initialParties = [
            { id: 1, title: "周末钱湖天地xx火锅拼4人车，已有2人，来吃货！", slots: 4, current: 2, date: "30分钟前" },
            { id: 2, title: "学府一号某烤肉店，实训周熬夜结束聚餐，求2个性格开朗的饭搭子", slots: 6, current: 4, date: "1小时前" }
        ];

        // --- 渲染页面逻辑 ---
        function renderData() {
            const area = document.getElementById('areaFilter').value;
            const search = document.getElementById('searchInput').value.toLowerCase();
            const container = document.getElementById('content-list');
            container.innerHTML = '';

            if (currentTab === 'reviews') {
                let filtered = initialReviews.filter(item => {
                    const matchArea = (area === 'all' || item.area === area);
                    const matchSearch = item.name.toLowerCase().includes(search);
                    return matchArea && matchSearch;
                });

                if(filtered.length === 0) container.innerHTML = '<p style="text-align:center;color:#999;">暂无相关美食排雷情报</p>';
                
                filtered.forEach(item => {
                    const isBlack = item.type === '黑榜避雷';
                    container.innerHTML += `
                        <div class="card ${isBlack ? 'black-list' : 'red-list'}">
                            <div class="card-header">
                                <div class="store-name">${item.name}</div>
                                <span class="badge ${isBlack ? 'red' : 'green'}">${isBlack ? '⚠️ 黑榜避雷' : '👍 红榜推荐'}</span>
                            </div>
                            <div class="meta-info">商圈：${item.area} | 人均：¥${item.price} | 发布：${item.date}</div>
                            <div class="review-text">“${item.text}”</div>
                        </div>
                    `;
                });
            } else {
                // 渲染饭搭子
                let filtered = initialParties.filter(item => item.title.toLowerCase().includes(search));
                if(filtered.length === 0) container.innerHTML = '<p style="text-align:center;color:#999;">目前还没有人在拼单，快去发起一个！</p>';
                
                filtered.forEach(item => {
                    container.innerHTML += `
                        <div class="card" style="border-left-color: var(--secondary-color)">
                            <div class="card-header">
                                <div class="store-name">${item.title}</div>
                                <span class="badge orange">🚗 组队中 (${item.current}/${item.slots})</span>
                            </div>
                            <div class="meta-info">发布时间：${item.date}</div>
                            <button class="submit-btn" style="width:auto; padding: 6px 15px; font-size:12px; margin-top:5px;" onclick="joinParty(${item.id})">一键申请加入</button>
                        </div>
                    `;
                });
            }
        }

        // --- 切换选项卡 ---
        function switchTab(tab) {
            currentTab = tab;
            document.getElementById('tab-reviews').classList.toggle('active', tab === 'reviews');
            document.getElementById('tab-parties').classList.toggle('active', tab === 'parties');
            
            // 切换发帖表单的显示
            document.getElementById('review-form-fields').style.display = tab === 'reviews' ? 'block' : 'none';
            document.getElementById('party-form-fields').style.display = tab === 'parties' ? 'block' : 'none';
            document.getElementById('form-title').innerText = tab === 'reviews' ? '我要发帖排雷/推荐' : '发起我的饭搭子拼单';
            
            renderData();
        }

        // --- 提交数据（增删改查之“增”） ---
        function handleSubmit() {
            if (currentTab === 'reviews') {
                const name = document.getElementById('form-store-name').value;
                const area = document.getElementById('form-area').value;
                const type = document.getElementById('form-type').value;
                const price = document.getElementById('form-price').value;
                const text = document.getElementById('form-content').value;

                if (!name || !text) return alert('店名和吐槽内容不能为空！');

                initialReviews.unshift({ id: Date.now(), name, area, type, price, text, date: "刚刚" });
                // 清空表单
                document.getElementById('form-store-name').value = '';
                document.getElementById('form-content').value = '';
                alert('🚀 发布成功！已同步至南高教园区新生雷达。');
            } else {
                const title = document.getElementById('form-party-title').value;
                const slots = parseInt(document.getElementById('form-party-slots').value);

                if (!title) return alert('拼单主题不能为空！');

                initialParties.unshift({ id: Date.now(), title, slots, current: 1, date: "刚刚" });
                document.getElementById('form-party-title').value = '';
                alert('🔥 发起成功！去通知你的室友吧。');
            }
            renderData();
        }

        // --- 加入拼单 ---
        function joinParty(id) {
            let party = initialParties.find(p => p.id === id);
            if (party.current >= party.slots) {
                alert('该车队已满员啦！');
            } else {
                party.current++;
                alert('申请成功！已将你的联系方式发送给队长。');
                renderData();
            }
        }

        // 首次加载初始化渲染
        renderData();
    </script>
</body>
</html>
