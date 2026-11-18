<p align="center"><a href="README.md">English</a> · <b>العربية</b></p>

<p align="center"><img src=".github/assets/banner.ar.svg" width="100%" alt="Elementor HTML to Page: من HTML لصفحة Elementor بأقل أخطاء"></p>

> **من HTML لصفحة Elementor بأقل أخطاء.** تحوّل ملف HTML أو تصميم جاهز لصفحة Draft في Elementor عن طريق Elementor MCP، بتطابق بصري حوالي 95% وبأقل استهلاك. بتبني التخطيط والمحتوى بس، ومابتنشرش الصفحة.

## بتشتغل إزاي؟

<p align="center"><img src=".github/assets/flow.ar.svg" width="100%" alt="الخطوات: ادّيها التصميم، قياس وتخطيط، البناء، صفحة Draft"></p>

## قولها كده وسيبها تشتغل

- «ابني ملف HTML ده كصفحة في Elementor»
- «حوّل التصميم ده لصفحة Elementor»

## النتايج (بهزر، بس بجد مفيدة)

<p align="center"><img src=".github/assets/chart.ar.svg" width="100%" alt="رسم فكاهي غير علمي. لفّات تعديل الموبايل: 90% من غيرها و20% معاها؛ التوكنز المحروقة: 85% من غيرها و30% معاها؛ الصفحة شبه التصميم: 45% من غيرها و95% معاها"></p>

## ركّبها في دقيقة

| بتستخدم إيه؟ | الطريقة |
|---|---|
| حساب Claude أو ChatGPT | نزّل [`elementor-html-to-page.zip`](https://github.com/Marsadgrouplabs/elementor-html-to-page/releases/latest/download/elementor-html-to-page.zip) وارفعه من قسم Skills |
| Claude Code | `claude plugin marketplace add Marsadgrouplabs/elementor-html-to-page` |
| Claude Code أو Codex بسكربت | حمّل الـ repo وشغّل `install.ps1` أو `install.sh` |

<details>
<summary><b>الخطوات بالتفصيل</b></summary>

### حساب Claude أو ChatGPT

1. نزّل [`elementor-html-to-page.zip`](https://github.com/Marsadgrouplabs/elementor-html-to-page/releases/latest/download/elementor-html-to-page.zip) وماتفكّهوش.
2. **Claude:** افتح قسم Skills في حسابك واختار رفع ملف.
3. **ChatGPT:** افتح Skills، ثم Create، ثم Upload from your computer. الميزة متاحة حسب خطة الحساب.

### Claude Code كـ Plugin

```bash
claude plugin marketplace add Marsadgrouplabs/elementor-html-to-page
claude plugin install elementor-html-to-page@marsad-elementor-html-to-page
```

### ويندوز

```powershell
git clone https://github.com/Marsadgrouplabs/elementor-html-to-page.git
cd elementor-html-to-page
powershell -ExecutionPolicy Bypass -File .\install.ps1
```

### ماك أو لينكس

```bash
git clone https://github.com/Marsadgrouplabs/elementor-html-to-page.git
cd elementor-html-to-page
bash install.sh
```

السكربت بيركّبها لـ Claude Code في `~/.claude/skills` ولـ Codex في `~/.agents/skills`، ولو فيه نسخة قديمة بيحتفظ بيها.

</details>

## مطمنك: مين الناشر وبياناتك فين؟

الناشر هو **[Marsad Labs](https://labs.marsadgroup.co)**. Marsad Labs هي ذراع المنتجات في Marsad Group، شركة تسويق مسجلة في بريطانيا، وبتبني أدوات عملية للمسوقين والمطورين. [اعرف أكتر عننا](https://labs.marsadgroup.co/about-us/) · [سياسة الخصوصية](https://labs.marsadgroup.co/privacy-policy/)

| السؤال | الإجابة |
|---|---|
| **بتقرا إيه؟** | ملف التصميم وبيانات الصفحة عن طريق Elementor MCP. |
| **بتنشر لوحدها؟** | لأ. الصفحة بتفضل Draft، وممنوع النشر أو الحذف من غير موافقتك. |
| **محتاجة مفاتيح؟** | بتستخدم اتصال Elementor MCP الموجود عندك. |
| **الكود** | تعليمات ومراجع نصية بس، ومفيهاش أي سكربتات بتشتغل على جهازك. |

النسخة دي اتفحصت بأداة SkillSpector، واتراجعت تنبيهاتها الحرجة والعالية يدوياً، واتشال منها أي بيانات عملاء أو مفاتيح. النتيجة الكاملة وطريقة التأكد من الملف والإبلاغ عن مشكلة في [`SECURITY.ar.md`](SECURITY.ar.md).

## محتاج إيه؟

- موقع WordPress عليه Elementor ومتوصل بـ Elementor MCP.

## الحقوق

كل الحقوق محفوظة لـ Marsad Labs. ممنوع النسخ أو إعادة النشر بدون إذن.

---

<p align="center">معمولة بحب وشاي كتير في <a href="https://labs.marsadgroup.co"><b>Marsad Labs</b></a></p>
