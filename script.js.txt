let tokens = [];
let points = 100;
let currentLang = 'zh';
let musicPlaying = false;
let currentUser = "Visitor" + Math.floor(Math.random() * 1000);

// 语言内容
const translations = {
    zh: {
        title: '垃圾代幣博物館',
        subtitle: '在此殿堂，定制並交易汝之“至廢珍寶”',
        createTitle: '鑄造廢幣',
        createBtn: '入館珍藏',
        marketTitle: '廢幣集市',
        hallTitle: '代幣大廳',
        myTokensTab: '我的藏品',
        allTokensTab: '公共展廳',
        tokenNamePlaceholder: '代幣名稱（例：廢紙幣）',
        tokenDescPlaceholder: '描述（例：一文不值）',
        pointsLabel: '汝之積分：',
        priceLabel: '價格：',
        ownerLabel: '持有者：',
        buyBtn: '購之',
        sellBtn: '售之',
        alertMissing: '請填寫名稱和描述！',
        alertOwnedOrPoor: '汝已擁有，或積分不足！',
        alertNotYours: '非汝之物也！',
        musicBtnOn: '音樂關',
        musicBtnOff: '音樂開',
        tips: [
            '汝可創建虛擬市場中最令汝痛恨之同名幣，此幣或將帶來巨大損失！',
            '謹慎，此廢幣或將成為集市中最無用之物！',
            '此幣或許能騙過新手，但老手一笑置之。',
            '恭喜，汝之垃圾傑作已入館，願它帶來歡笑而非財富！',
            '此幣若真有價值，恐是天方夜譚也！'
        ]
    },
    en: {
        title: 'Worst Token Museum',
        subtitle: 'Craft and trade your "most worthless treasures" here',
        createTitle: 'Create a Trash Token',
        createBtn: 'Add to Collection',
        marketTitle: 'Token Market',
        hallTitle: 'Token Hall',
        myTokensTab: 'My Collection',
        allTokensTab: 'Public Gallery',
        tokenNamePlaceholder: 'Token Name (e.g., Junk Coin)',
        tokenDescPlaceholder: 'Description (e.g., Worthless)',
        pointsLabel: 'Your Points: ',
        priceLabel: 'Price: ',
        ownerLabel: 'Owner: ',
        buyBtn: 'Buy',
        sellBtn: 'Sell',
        alertMissing: 'Please fill in name and description!',
        alertOwnedOrPoor: 'You already own it or lack points!',
        alertNotYours: 'This is not yours!',
        musicBtnOn: 'Music Off',
        musicBtnOff: 'Music On',
        tips: [
            'You can create a namesake of the token you hate most in the virtual market—it might bring you huge losses!',
            'Beware, this trash token might become the most useless item in the market!',
            'This coin might fool newbies, but veterans will just laugh it off.',
            'Congrats, your trash masterpiece is now in the museum—may it bring laughter, not riches!',
            'If this coin ever gains value, it’d be a miracle!'
        ]
    }
};

// 创建代币
function createToken() {
    const name = document.getElementById("tokenName").value;
    const desc = document.getElementById("tokenDesc").value;
    
    if (name && desc) {
        const token = {
            id: tokens.length + 1,
            name: name,
            desc: desc,
            price: Math.floor(Math.random() * 50) + 1,
            owner: currentLang === 'zh' ? '博物館' : 'Museum',
            creator: currentUser
        };
        tokens.push(token);
        document.getElementById("tokenName").value = "";
        document.getElementById("tokenDesc").value = "";
        
        const tips = translations[currentLang].tips;
        const randomTip = tips[Math.floor(Math.random() * tips.length)];
        alert(randomTip);
        
        updateMarket();
        updateHall();
    } else {
        alert(translations[currentLang].alertMissing);
    }
}

// 更新市场
function updateMarket() {
    const market = document.getElementById("market");
    market.innerHTML = `<p>${translations[currentLang].pointsLabel}${points}</p>`;
    
    tokens.forEach(token => {
        market.innerHTML += `
            <div class="token">
                <strong>${token.name}</strong><br>
                ${token.desc}<br>
                ${translations[currentLang].priceLabel}${token.price} Points<br>
                ${translations[currentLang].ownerLabel}${token.owner}<br>
                <button onclick="buyToken(${token.id})">${translations[currentLang].buyBtn}</button>
                <button onclick="sellToken(${token.id})">${translations[currentLang].sellBtn}</button>
            </div>
        `;
    });
}

// 更新大厅
function updateHall() {
    const myTokens = document.getElementById("my-tokens");
    const allTokens = document.getElementById("all-tokens");
    const ownerYou = currentLang === 'zh' ? '你' : 'You';
    
    myTokens.innerHTML = `<p>${translations[currentLang].pointsLabel}${points}</p>`;
    allTokens.innerHTML = '';
    
    tokens.filter(token => token.owner === ownerYou || token.creator === currentUser).forEach(token => {
        myTokens.innerHTML += `
            <div class="token">
                <strong>${token.name}</strong><br>
                ${token.desc}<br>
                ${translations[currentLang].priceLabel}${token.price} Points<br>
                ${translations[currentLang].ownerLabel}${token.owner}<br>
                ${translations[currentLang].ownerLabel === '持有者：' ? '創作者：' : 'Creator: '}${token.creator}
            </div>
        `;
    });
    
    tokens.forEach(token => {
        allTokens.innerHTML += `
            <div class="token">
                <strong>${token.name}</strong><br>
                ${token.desc}<br>
                ${translations[currentLang].priceLabel}${token.price} Points<br>
                ${translations[currentLang].ownerLabel}${token.owner}<br>
                ${translations[currentLang].ownerLabel === '持有者：' ? '創作者：' : 'Creator: '}${token.creator}
            </div>
        `;
    });
}

// 购买代币
function buyToken(id) {
    const token = tokens.find(t => t.id === id);
    const ownerYou = currentLang === 'zh' ? '你' : 'You';
    if (token.owner === ownerYou || points < token.price) {
        alert(translations[currentLang].alertOwnedOrPoor);
    } else {
        points -= token.price;
        token.owner = ownerYou;
        updateMarket();
        updateHall();
    }
}

// 出售代币
function sellToken(id) {
    const token = tokens.find(t => t.id === id);
    const ownerYou = currentLang === 'zh' ? '你' : 'You';
    if (token.owner !== ownerYou) {
        alert(translations[currentLang].alertNotYours);
    } else {
        points += token.price;
        token.owner = currentLang === 'zh' ? '博物館' : 'Museum';
        updateMarket();
        updateHall();
    }
}

// 切换页面
function showSection(section) {
    document.getElementById("create-section").style.display = section === 'create' ? 'block' : 'none';
    document.getElementById("market-section").style.display = section === 'market' ? 'block' : 'none';
    document.getElementById("hall-section").style.display = section === 'hall' ? 'block' : 'none';
    if (section === 'market') updateMarket();
    if (section === 'hall') updateHall();
}

// 切换大厅标签
function showHallTab(tab) {
    document.getElementById("my-tokens").style.display = tab === 'my-tokens' ? 'block' : 'none';
    document.getElementById("all-tokens").style.display = tab === 'all-tokens' ? 'block' : 'none';
}

// 切换语言
function changeLanguage() {
    currentLang = document.getElementById("language").value;
    
    // 更新静态文本
    document.getElementById("title").textContent = translations[currentLang].title;
    document.getElementById("subtitle").textContent = translations[currentLang].subtitle;
    document.getElementById("create-title").textContent = translations[currentLang].createTitle;
    document.getElementById("create-btn").textContent = translations[currentLang].createBtn;
    document.getElementById("market-title").textContent = translations[currentLang].marketTitle;
    document.getElementById("hall-title").textContent = translations[currentLang].hallTitle;
    document.getElementById("my-tokens-tab").textContent = translations[currentLang].myTokensTab;
    document.getElementById("all-tokens-tab").textContent = translations[currentLang].allTokensTab;
    document.getElementById("tokenName").placeholder = translations[currentLang].tokenNamePlaceholder;
    document.getElementById("tokenDesc").placeholder = translations[currentLang].tokenDescPlaceholder;
    document.getElementById("nav-create").textContent = translations[currentLang].createTitle;
    document.getElementById("nav-market").textContent = translations[currentLang].marketTitle;
    document.getElementById("nav-hall").textContent = translations[currentLang].hallTitle;
    
    // 更新动态内容
    updateMusicButton();
    updateMarket();
    updateHall();
}

// 控制背景音乐
function toggleMusic() {
    const audio = document.getElementById("bgMusic");
    if (musicPlaying) {
        audio.pause();
        musicPlaying = false;
    } else {
        audio.play();
        musicPlaying = true;
    }
    updateMusicButton();
}

function updateMusicButton() {
    document.getElementById("music-btn").textContent = musicPlaying ? translations[currentLang].musicBtnOn : translations[currentLang].musicBtnOff;
}

// 初始化
showSection('create');
showHallTab('my-tokens');
changeLanguage();