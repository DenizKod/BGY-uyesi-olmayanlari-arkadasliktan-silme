# FacebookBGY

# ADIM 1 (BETİK 1)

<p>- Tampermonkey eklentisini tarayıcına ekle</p>
<p>- Yeni Betik oluştur (3 adet ayrı betik oluşturacağız ama şimdilik 1 tane oluşturun)</p>
<p>- Aşağıdaki kodu kopyalayın ve betik oluşturma sayfasına yapıştırın (betiğin içinde "dosya" yazan yere tıklayıp kaydet basın)</p>
<p>- Bu adrese git : https://www.facebook.com/groups/bgyagain3/members/friends</p>
<p>- Listenin sonuna gidene kadar en aşağı kaydırın. ardından listenin ortasına sayfa kapanmayacak şekilde 1 kere tıklayın. f8 tuşuna basın.</p>
<p>- Bir metin belgesi açın Ctrl + V ile isimleri yapıştırın. lisenin en başındaki ismi bulun ardından onun gerisindeki tüm gereksiz yazıları silin. listede sadece isimlerin virgül virgül ayrı ayrı yazdığına emin olun.</p>

**ÜYELERİ TOPLAMA BETİĞİ KODU**
```
// ==UserScript==
// @name         Facebook Üye İsim Kopyalayıcı
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Facebook grubunun üye listesindeki isimleri kopyalamak için kullanılır. F8 tuşuna basarak isimleri kopyalar. F10 tuşuna basarak kopyalanan isimleri sıfırlar.
// @author       Deniz KOD
// @match        https://www.facebook.com/groups/bgyagain3/members/friends
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    let copiedNames = []; // Kopyalanan isimleri saklamak için dizi

    // F8 tuşuna basıldığında isimleri kopyala
    window.addEventListener('keydown', function(e) {
        if (e.key === 'F8') {
            e.preventDefault(); // Tarayıcının varsayılan işlevini engelle
            copyNames();
        } else if (e.key === 'F10') {
            e.preventDefault(); // Tarayıcının varsayılan işlevini engelle
            resetCopiedNames();
        }
    });

    // İsimleri kopyala
    function copyNames() {
        let nameElements = document.querySelectorAll('div[data-visualcompletion="ignore-dynamic"] div div a[aria-label]');

        if (nameElements.length === 0) {
            alert('Üye bulunamadı!');
            return;
        }

        copiedNames = []; // Önceki kopyalanan isimleri temizle

        nameElements.forEach(function(element) {
            let name = element.getAttribute('aria-label').trim();
            copiedNames.push(name);
        });

        copyToClipboard(copiedNames.join(', '));
        alert('Başarıyla kopyalandı!');
    }

    // Kopyalanan isimleri panoya kopyala
    function copyToClipboard(text) {
        const input = document.createElement('textarea');
        input.value = text;
        document.body.appendChild(input);
        input.select();
        document.execCommand('copy');
        document.body.removeChild(input);
    }

    // Kopyalanan isimleri sıfırla
    function resetCopiedNames() {
        if (copiedNames.length === 0) {
            alert('Sıfırlanacak isim bulunamadı!');
            return;
        }

        copiedNames = [];
        alert('Başarıyla isimler sıfırlandı!');
    }
})();
```

# 2. ADIM

<p>- Kopyaladığın isimleri şimdi bu kodun içerisine ekleyeceğiz.</p>
<p>- Teni betik oluşturun</p>
<p>- 2. betiğin kodunda bu kısmı bulun : const gizlenecekKullaniciAdlariString = "Sefa Morgan, エレ ン, Cihangir Hürremi, Atom Ali"; ARDINDAN ve sizin kopyaladığınız virgülle ayrılmış isimleri "isim, isim, isim" çift tırnağın arasına ekleyin ve kaydedin</p>
<p>- Profilinize gidip arkadaşlarınıza girin en alta kadar inin. en alta indiğinizde sayfanın ortasına 1 kere kapanmayacak şekilde tıklayın. F6 tuşuna basın.</p>
<p>- Lisedeki isimler silindiyse 3. betiğe geçebiliriz</p>

```
// ==UserScript==
// @name         Facebook Profil Kartı Kaldırma
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Facebook'ta belirtilen isimlere sahip kullanıcıların profil kartlarını DOM'dan kaldırır.
// @author       Deniz KOD
// @match        https://www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Tüm isimler tek bir string içinde, virgülle ayrılmış şekilde
    const gizlenecekKullaniciAdlariString = "Sefa Morgan, エレ ン, Cihangir Hürremi, Atom Ali";
    // String'i virgülle ayırarak diziye dönüştür
    const gizlenecekKullaniciAdlari = gizlenecekKullaniciAdlariString.split(", ");

    // F6 tuşuna basıldığında silme işlemi başlat
    document.addEventListener('keydown', function(e) {
        if (e.key === 'F6') {
            const profilKartlari = document.querySelectorAll('div[class*="x1lq5wgf"]:not([data-visualcompletion="ignore-dynamic"])');
            profilKartlari.forEach(kart => {
                const isimElementi = kart.querySelector('span[dir="auto"]');
                if (isimElementi && gizlenecekKullaniciAdlari.includes(isimElementi.innerText.trim())) {
                    kart.remove(); // Elementi DOM'dan tamamen kaldır
                }
            });
        }
    });
})();
```


# KISIM 3 (BETİK 3)

<p>- BGY'deki herkesin listede kaldığına emin olduysanız aşağıdaki 3. betiği de eklemeye hazırsınız</p>
<p>- F2 ile silmeye başlar sırayla.</p>

**ARKADAŞLARI OTOMATİK SİLME BETİĞİ (BETİK 3)**

```

// ==UserScript==
// @name         Arkadaş Listesinden Otomatik Silme
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  BGY dışı herkesi silme
// @author       Deniz KOD
// @match        https://www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

   document.addEventListener('keydown', function(e) {
    if (e.key === 'F2') {
        console.log("Silme işlemi başlatıldı.");
        startRemovingFriends();
    } else if (e.key === 'F4') {
        console.log("Silme işlemi durduruldu.");
        stopRemovingFriends();
    }
});

let isRunning = false;

function startRemovingFriends() {
    isRunning = true;
    removeNextFriend();
}

function stopRemovingFriends() {
    isRunning = false;
}

function removeNextFriend() {
    if (!isRunning) return;

    const friendButton = document.querySelector('div[aria-label="Arkadaşlar"]');
    if (friendButton) {
        friendButton.click();

        setTimeout(() => {
            const allMenuItems = Array.from(document.querySelectorAll('[role="menuitem"]'));
            const removeOption = allMenuItems.find(item => {
                const label = item.textContent || item.innerText;
                return label.includes("Arkadaşlarımdan Çıkar");
            });

            if (removeOption) {
                removeOption.click();
                console.log("Arkadaşlarımdan Çıkar seçeneği tıklandı.");
                setTimeout(confirmDeletion, 500);  // Ekstra zaman, menü kapanmasını beklemek için
            } else {
                console.log("Arkadaşlarımdan Çıkar seçeneği bulunamadı.");
                setTimeout(removeNextFriend, 1500);  // Bir sonraki arkadaşı denemek için
            }
        }, 1000); // Menü açılmasını beklemek için süre
    } else {
        console.log("Daha fazla arkadaş butonu bulunamadı.");
    }
}

function confirmDeletion() {
    const confirmButton = document.querySelector('div[aria-label="Onayla"]');
    if (confirmButton) {
        confirmButton.click();
        console.log("Onayla butonuna basıldı.");
        setTimeout(() => {
            removeNextFriend();  // Sonraki silme işlemine geç
        }, 100);  // Onaydan sonra kısa bir gecikme
    } else {
        console.log("Onayla butonu bulunamadı.");
        setTimeout(removeNextFriend, 1500);  // Yine denemek için bir gecikme
    }
}
})();
```