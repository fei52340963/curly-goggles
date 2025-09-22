<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>身份证号AI解码器</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    
    <!-- 配置Tailwind自定义颜色和字体 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#3B82F6',
                        secondary: '#10B981',
                        neutral: '#64748B',
                        light: '#F1F5F9',
                        dark: '#1E293B'
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .card-shadow {
                box-shadow: 0 10px 25px -5px rgba(59, 130, 246, 0.1), 0 8px 10px -6px rgba(59, 130, 246, 0.1);
            }
            .input-focus {
                @apply focus:ring-2 focus:ring-primary/50 focus:border-primary focus:outline-none;
            }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-light to-blue-50 min-h-screen font-sans text-dark">
    <div class="container mx-auto px-4 py-12 max-w-4xl">
        <!-- 页面标题 -->
        <header class="text-center mb-12">
            <h1 class="text-[clamp(2rem,5vw,3rem)] font-bold text-dark mb-4 flex items-center justify-center">
                <i class="fa fa-id-card text-primary mr-3"></i>
                身份证号AI解码器
            </h1>
            <p class="text-neutral text-lg max-w-2xl mx-auto">
                输入18位身份证号码，系统将自动解析出地址码、出生日期码、顺序码和校验码
            </p>
        </header>
        
        <!-- 主内容区 -->
        <main class="bg-white rounded-2xl card-shadow p-6 md:p-8">
            <!-- 输入区域 -->
            <div class="mb-8">
                <label for="idCard" class="block text-sm font-medium text-neutral mb-2">
                    请输入18位身份证号码
                </label>
                <div class="flex flex-col sm:flex-row gap-4">
                    <input 
                        type="text" 
                        id="idCard" 
                        placeholder="例如：110101199001011234" 
                        class="flex-1 px-4 py-3 border border-gray-300 rounded-lg input-focus transition-all duration-300"
                        maxlength="18"
                    >
                    <button 
                        id="decodeBtn" 
                        class="bg-primary hover:bg-primary/90 text-white px-6 py-3 rounded-lg font-medium transition-all duration-300 transform hover:scale-105 flex items-center justify-center gap-2"
                    >
                        <i class="fa fa-search"></i>
                        <span>解析身份证号</span>
                    </button>
                </div>
                <p id="errorMsg" class="text-red-500 text-sm mt-2 hidden"></p>
            </div>
            
            <!-- 结果展示区域 -->
            <div id="resultSection" class="hidden opacity-0 transition-opacity duration-500">
                <div class="border-t border-gray-200 pt-6 mb-6">
                    <h2 class="text-xl font-semibold mb-4 flex items-center">
                        <i class="fa fa-bar-chart text-primary mr-2"></i>
                        解析结果
                    </h2>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- 地址码 -->
                    <div class="bg-light rounded-xl p-5 hover:shadow-md transition-shadow duration-300">
                        <div class="flex items-start mb-2">
                            <div class="bg-primary/10 p-2 rounded-lg mr-3">
                                <i class="fa fa-map-marker text-primary text-xl"></i>
                            </div>
                            <h3 class="font-medium text-lg">地址码</h3>
                        </div>
                        <p id="areaCode" class="text-dark ml-10"></p>
                        <p id="areaInfo" class="text-neutral text-sm mt-1 ml-10"></p>
                    </div>
                    
                    <!-- 出生日期码 -->
                    <div class="bg-light rounded-xl p-5 hover:shadow-md transition-shadow duration-300">
                        <div class="flex items-start mb-2">
                            <div class="bg-primary/10 p-2 rounded-lg mr-3">
                                <i class="fa fa-calendar text-primary text-xl"></i>
                            </div>
                            <h3 class="font-medium text-lg">出生日期码</h3>
                        </div>
                        <p id="birthCode" class="text-dark ml-10"></p>
                        <p id="birthDate" class="text-neutral text-sm mt-1 ml-10"></p>
                    </div>
                    
                    <!-- 顺序码 -->
                    <div class="bg-light rounded-xl p-5 hover:shadow-md transition-shadow duration-300">
                        <div class="flex items-start mb-2">
                            <div class="bg-primary/10 p-2 rounded-lg mr-3">
                                <i class="fa fa-list-ol text-primary text-xl"></i>
                            </div>
                            <h3 class="font-medium text-lg">顺序码</h3>
                        </div>
                        <p id="sequenceCode" class="text-dark ml-10"></p>
                        <p id="genderInfo" class="text-neutral text-sm mt-1 ml-10"></p>
                    </div>
                    
                    <!-- 校验码 -->
                    <div class="bg-light rounded-xl p-5 hover:shadow-md transition-shadow duration-300">
                        <div class="flex items-start mb-2">
                            <div class="bg-primary/10 p-2 rounded-lg mr-3">
                                <i class="fa fa-check-circle text-primary text-xl"></i>
                            </div>
                            <h3 class="font-medium text-lg">校验码</h3>
                        </div>
                        <p id="checkCode" class="text-dark ml-10"></p>
                        <p class="text-neutral text-sm mt-1 ml-10">用于验证身份证号码的正确性</p>
                    </div>
                </div>
            </div>
        </main>
        
        <!-- 信息说明 -->
        <div class="mt-10 bg-white rounded-xl p-6 card-shadow">
            <h3 class="text-lg font-semibold mb-3 flex items-center">
                <i class="fa fa-info-circle text-primary mr-2"></i>
                身份证号码组成说明
            </h3>
            <ul class="space-y-2 text-neutral">
                <li class="flex items-start">
                    <i class="fa fa-circle text-xs text-primary mt-1.5 mr-2"></i>
                    <span><strong>地址码(前6位)</strong>：表示登记户口时所在地的行政区划代码</span>
                </li>
                <li class="flex items-start">
                    <i class="fa fa-circle text-xs text-primary mt-1.5 mr-2"></i>
                    <span><strong>出生日期码(第7-14位)</strong>：表示出生的年、月、日</span>
                </li>
                <li class="flex items-start">
                    <i class="fa fa-circle text-xs text-primary mt-1.5 mr-2"></i>
                    <span><strong>顺序码(第15-17位)</strong>：表示在同一地址码所标识的区域范围内，对同年、同月、同日出生的人编定的顺序号，其中第17位奇数分配给男性，偶数分配给女性</span>
                </li>
                <li class="flex items-start">
                    <i class="fa fa-circle text-xs text-primary mt-1.5 mr-2"></i>
                    <span><strong>校验码(第18位)</strong>：根据前面十七位数字码，按照ISO 7064:1983.MOD 11-2校验码计算出来的检验码</span>
                </li>
            </ul>
        </div>
        
        <!-- 页脚 -->
        <footer class="mt-12 text-center text-neutral text-sm">
            <p>身份证号AI解码器 &copy; 2023 | 仅用于学习和演示</p>
        </footer>
    </div>

    <script>
        // 地址码映射表：包含所有省级代码和主要城市代码
        const areaCodeMap = {
            // 华北地区
            '110000': '北京市',
            '110101': '北京市东城区',
            '110102': '北京市西城区',
            '110105': '北京市朝阳区',
            '110106': '北京市丰台区',
            
            '120000': '天津市',
            '120101': '天津市和平区',
            '120102': '天津市河东区',
            
            '130000': '河北省',
            '130100': '河北省石家庄市',
            '130200': '河北省唐山市',
            '130300': '河北省秦皇岛市',
            
            '140000': '山西省',
            '140100': '山西省太原市',
            '140200': '山西省大同市',
            
            '150000': '内蒙古自治区',
            '150100': '内蒙古自治区呼和浩特市',
            '150200': '内蒙古自治区包头市',
            
            // 东北地区
            '210000': '辽宁省',
            '210100': '辽宁省沈阳市',
            '210200': '辽宁省大连市',
            
            '220000': '吉林省',
            '220100': '吉林省长春市',
            '220200': '吉林省吉林市',
            
            '230000': '黑龙江省',
            '230100': '黑龙江省哈尔滨市',
            '230200': '黑龙江省齐齐哈尔市',
            
            // 华东地区
            '310000': '上海市',
            '310101': '上海市黄浦区',
            '310104': '上海市徐汇区',
            
            '320000': '江苏省',
            '320100': '江苏省南京市',
            '320200': '江苏省无锡市',
            '320500': '江苏省苏州市',
            
            '330000': '浙江省',
            '330100': '浙江省杭州市',
            '330200': '浙江省宁波市',
            '330300': '浙江省温州市',
            
            '340000': '安徽省',
            '340100': '安徽省合肥市',
            '340200': '安徽省芜湖市',
            
            '350000': '福建省',
            '350100': '福建省福州市',
            '350200': '福建省厦门市',
            '352200': '福建省宁德地区',
            '352202': '福建省宁德地区福安市',
            
            '360000': '江西省',
            '360100': '江西省南昌市',
            '360700': '江西省赣州市',
            
            '370000': '山东省',
            '370100': '山东省济南市',
            '370200': '山东省青岛市',
            
            // 中南地区
            '410000': '河南省',
            '410100': '河南省郑州市',
            '410300': '河南省洛阳市',
            
            '420000': '湖北省',
            '420100': '湖北省武汉市',
            '420500': '湖北省宜昌市',
            
            '430000': '湖南省',
            '430100': '湖南省长沙市',
            '430200': '湖南省株洲市',
            
            '440000': '广东省',
            '440100': '广东省广州市',
            '440300': '广东省深圳市',
            '440400': '广东省珠海市',
            '440500': '广东省汕头市',
            
            '450000': '广西壮族自治区',
            '450100': '广西壮族自治区南宁市',
            '450200': '广西壮族自治区柳州市',
            
            '460000': '海南省',
            '460100': '海南省海口市',
            '460200': '海南省三亚市',
            
            // 西南地区
            '500000': '重庆市',
            '500101': '重庆市万州区',
            '500102': '重庆市涪陵区',
            
            '510000': '四川省',
            '510100': '四川省成都市',
            '510300': '四川省自贡市',
            
            '520000': '贵州省',
            '520100': '贵州省贵阳市',
            '520200': '贵州省六盘水市',
            
            '530000': '云南省',
            '530100': '云南省昆明市',
            '530300': '云南省曲靖市',
            
            '540000': '西藏自治区',
            '540100': '西藏自治区拉萨市',
            
            // 西北地区
            '610000': '陕西省',
            '610100': '陕西省西安市',
            '610200': '陕西省铜川市',
            
            '620000': '甘肃省',
            '620100': '甘肃省兰州市',
            '620200': '甘肃省嘉峪关市',
            
            '630000': '青海省',
            '630100': '青海省西宁市',
            
            '640000': '宁夏回族自治区',
            '640100': '宁夏回族自治区银川市',
            
            '650000': '新疆维吾尔自治区',
            '650100': '新疆维吾尔自治区乌鲁木齐市',
            
            // 特别行政区
            '810000': '香港特别行政区',
            '820000': '澳门特别行政区',
            '710000': '台湾省'
            
            // 注意：完整的地址码需要包含所有地市级和区县级代码
            // 这里只列出了部分主要城市代码作为示例
        };

        // 计算校验码的函数
        function calculateCheckCode(idCardBody) {
            const factors = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
            const checkCodes = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'];
            
            let sum = 0;
            for (let i = 0; i < 17; i++) {
                sum += parseInt(idCardBody[i]) * factors[i];
            }
            
            const remainder = sum % 11;
            return checkCodes[remainder];
        }

        // 验证身份证号格式
        function validateIdCard(idCard) {
            // 18位数字，最后一位可以是X或x
            const regex = /^\d{17}[\dXx]$/;
            if (!regex.test(idCard)) {
                return { valid: false, message: '请输入有效的18位身份证号码' };
            }
            
            // 验证校验码
            const body = idCard.substring(0, 17);
            const checkCode = idCard.substring(17).toUpperCase();
            const calculatedCheckCode = calculateCheckCode(body);
            
            if (checkCode !== calculatedCheckCode) {
                return { valid: false, message: '身份证号码校验码不正确，可能是无效号码' };
            }
            
            // 验证出生日期
            const year = parseInt(idCard.substring(6, 10));
            const month = parseInt(idCard.substring(10, 12));
            const day = parseInt(idCard.substring(12, 14));
            
            if (month < 1 || month > 12) {
                return { valid: false, message: '身份证号码中的月份无效' };
            }
            
            const daysInMonth = new Date(year, month, 0).getDate();
            if (day < 1 || day > daysInMonth) {
                return { valid: false, message: '身份证号码中的日期无效' };
            }
            
            return { valid: true };
        }

        // 解析身份证号
        function decodeIdCard(idCard) {
            // 转换为大写（处理X）
            idCard = idCard.toUpperCase();
            
            // 提取各部分
            const areaCode = idCard.substring(0, 6);
            const birthCode = idCard.substring(6, 14);
            const sequenceCode = idCard.substring(14, 17);
            const checkCode = idCard.substring(17);
            
            // 解析出生日期
            const year = birthCode.substring(0, 4);
            const month = birthCode.substring(4, 6);
            const day = birthCode.substring(6, 8);
            const birthDate = `${year}年${month}月${day}日`;
            
            // 解析性别
            const genderCode = parseInt(sequenceCode.substring(2));
            const gender = genderCode % 2 === 1 ? '男性' : '女性';
            
            // 获取地区信息
            let areaInfo = '未知地区';
            if (areaCodeMap[areaCode]) {
                areaInfo = areaCodeMap[areaCode];
            } else if (areaCodeMap[areaCode.substring(0, 4) + '00']) {
                areaInfo = areaCodeMap[areaCode.substring(0, 4) + '00'] + '（具体区县未知）';
            } else if (areaCodeMap[areaCode.substring(0, 2) + '0000']) {
                areaInfo = areaCodeMap[areaCode.substring(0, 2) + '0000'] + '（具体地区未知）';
            }
            
            return {
                areaCode,
                areaInfo,
                birthCode,
                birthDate,
                sequenceCode,
                gender,
                checkCode
            };
        }

        // 显示解析结果
        function displayResult(result) {
            document.getElementById('areaCode').textContent = result.areaCode;
            document.getElementById('areaInfo').textContent = result.areaInfo;
            
            document.getElementById('birthCode').textContent = result.birthCode;
            document.getElementById('birthDate').textContent = result.birthDate;
            
            document.getElementById('sequenceCode').textContent = result.sequenceCode;
            document.getElementById('genderInfo').textContent = `性别：${result.gender}`;
            
            document.getElementById('checkCode').textContent = result.checkCode;
            
            // 显示结果区域并添加动画
            const resultSection = document.getElementById('resultSection');
            resultSection.classList.remove('hidden');
            setTimeout(() => {
                resultSection.classList.remove('opacity-0');
            }, 10);
        }

        // 绑定事件
        document.addEventListener('DOMContentLoaded', () => {
            const idCardInput = document.getElementById('idCard');
            const decodeBtn = document.getElementById('decodeBtn');
            const errorMsg = document.getElementById('errorMsg');
            
            // 点击解析按钮
            decodeBtn.addEventListener('click', () => {
                const idCard = idCardInput.value.trim();
                errorMsg.classList.add('hidden');
                
                if (!idCard) {
                    errorMsg.textContent = '请输入身份证号码';
                    errorMsg.classList.remove('hidden');
                    return;
                }
                
                // 验证身份证号
                const validation = validateIdCard(idCard);
                if (!validation.valid) {
                    errorMsg.textContent = validation.message;
                    errorMsg.classList.remove('hidden');
                    return;
                }
                
                // 解析并显示结果
                const result = decodeIdCard(idCard);
                displayResult(result);
                
                // 滚动到结果区域
                document.getElementById('resultSection').scrollIntoView({
                    behavior: 'smooth',
                    block: 'start'
                });
            });
            
            // 支持回车键解析
            idCardInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    decodeBtn.click();
                }
            });
            
            // 输入时自动转换为大写（处理X）
            idCardInput.addEventListener('input', (e) => {
                e.target.value = e.target.value.toUpperCase();
            });
        });
    </script>
</body>
</html>
    
