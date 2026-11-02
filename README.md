# 🚗 California Auto Body Lead Generation Master Pipeline (10,000+ Leads)

একটি সম্পূর্ণ স্বয়ংক্রিয় এবং ফুল-কন্ট্রোলযুক্ত B2B লিড জেনারেশন পাইপলাইন। এটি পুরো ক্যালিফোর্নিয়া (CA) রাজ্যের ১৯০টি প্রধান শহর ও মেট্রোপলিটন এলাকার সব ইন্ডিপেন্ডেন্ট অটো বডি ও কলিশন রিপেয়ার শপের তথ্য Google Maps থেকে স্ক্র্যাপ করে এবং **টু-স্ট্রিম (Two-Stream)** আউটপুট তৈরি করে:

1. **📧 স্ট্রিম ১ (কোল্ড ইমেইলিং লিড):** যাদের অফিশিয়াল ওয়েবসাইট আছে তাদের ওয়েবসাইট থেকে ইমেইল বের করে, ডোমেইন ও MX রেকর্ড ভেরিফাই করে এবং **Instantly.ai** বা **Smartlead.ai**-তে সরাসরি আপলোড উপযোগী ক্লিন CSV তৈরি করে (`campaign_ready_leads.csv`)।
2. **📞 স্ট্রিম ২ (ওয়েব ডেভেলপমেন্ট সার্ভিস কোল্ড কলিং লিড):** গুগল ম্যাপসে যেসব অটো শপের কোনো ওয়েবসাইট নেই, তাদেরকে আলাদা ফাইলে সংরক্ষণ করে (`no-website.csv`), যাতে আপনার সেলস টিম সরাসরি ফোন করে ওয়েবসাইট তৈরি ও লোকাল এসইও (Web Development & SEO) সার্ভিস সেল করতে পারে!

*(পূর্ববর্তী টেক্সাস (TX)-এর সকল লিড ও ডাটা নিরাপদে `tx_archive/` ফোল্ডারে সংরক্ষিত আছে)*

---

## 🏗️ টু-স্ট্রিম আর্কিটেকচার (Pipeline Architecture)

```
                       [ 🔎 Google Maps Scraping ]
                     (১৯০টি ক্যালিফোর্নিয়া শহর x ৪ কি-ওয়ার্ড)
                                   │
          ┌────────────────────────┴────────────────────────┐
          ▼                                                 ▼
[ 🌐 যাদের নিজস্ব ওয়েবসাইট আছে ]                   [ 📵 যাদের কোনো ওয়েবসাইট নেই ]
          │                                                 │
          ▼                                                 ▼
     shops.csv                                        no-website.csv
          │                                        (সরাসরি সেলস কল দেওয়ার জন্য:
          ▼                                         ফোন, রেটিং ও ক্লিকযোগ্য ম্যাপস লিঙ্ক)
   email_spider.py (Scrapy ক্রলার)
          │
          ▼
   verify_emails.py (DNS & MX ভেরিফিকেশন)
          │
          ▼
   clean_leads.py (v2.2 হিউম্যান নেইম & টাইটেল কেস)
          │
          ▼
🎯 campaign_ready_leads.csv
(Instantly.ai / Smartlead.ai-এর জন্য প্রস্তুত)
```

---

## 📁 আউটপুট ফাইল ও কলাম স্পেসিফিকেশন

### ১. `campaign_ready_leads.csv` (কোল্ড ইমেইল ক্যাম্পেইনের জন্য)
| কলাম | উদাহরণ | বিবরণ |
|---|---|---|
| **First Name** | *John* | যাচাইকৃত হিউম্যান ফার্স্ট নেম (না থাকলে ব্ল্যাঙ্ক) |
| **Company Name** | *Pacific Coast Auto Body* | ন্যাচারাল Title Case ও এসইও ট্যাগলাইনমুক্ত নাম |
| **Email** | *john@pacificcoastautobody.com* | MX রেকর্ড ভেরিফাইড অ্যাক্টিভ বিজনেস ইমেইল |
| **Website** | *https://pacificcoastautobody.com* | শপের অফিশিয়াল ওয়েবসাইট |
| **Phone** | *+1 949-555-0199* | ইউএস ফরম্যাটের ফোন নম্বর |
| **Address** | *123 Main St* | শপের ফিজিক্যাল ঠিকানা |
| **City** | *Newport Beach* | ক্যালিফোর্নিয়ার শহর |
| **State** | *CA* | স্টেট কোড |

### ২. `no-website.csv` (ওয়েব ডেভেলপমেন্ট কোল্ড কলিংয়ের জন্য)
| কলাম | উদাহরণ | কোল্ড কলিংয়ে ব্যবহারের সুবিধা |
|---|---|---|
| **name** | *ODIN AUTOBODY AND PAINT* | শপের আসল নাম ধরে কথা শুরু করার জন্য |
| **phone** | *+1 949-722-8980* | সরাসরি শপ ওনার বা ম্যানেজারের সাথে যোগাযোগের নম্বর |
| **address** | *1685 Babcock St # C* | শপের লোকাল লোকেশন যাচাই করতে |
| **city / state** | *Newport Beach, CA* | লোকাল রেফারেন্স দিতে |
| **rating** | *4.8* | শক্তিশালী পিচিং পয়েন্ট: *"আপনার ৪.৮ স্টার রেটিং আছে, কিন্তু ওয়েবসাইট নেই!"* |
| **google_maps_url** | *https://www.google.com/maps/place/...* | ১ ক্লিকেই শপের ছবি, কাজের নমুনা ও প্রোফাইল দেখতে |

---

## 🎯 লিড কোয়ালিটি ও অ্যান্টি-স্প্যাম ফিল্টার

কোল্ড ইমেইলিং এবং কোল্ড কলিংয়ের সর্বোচ্চ কনভার্শন রেট ধরে রাখতে নিচের ফিল্টারগুলো সক্রিয় রয়েছে:

1. **নন-অটো স্প্যাম ফিল্টারিং (Spam / Clinic / Spa Blocker):**
   - গুগলের অ্যালগরিদম "Auto Body"-র সাথে "Body" শব্দ মিলিয়ে যেসব ম্যাসাজ পার্লার, বিউটি সেলুন, নেইল স্পা, ওয়াক্স বার বা কসমেটিক ক্লিনিক এনেছিল—সেগুলোকে স্বয়ংক্রিয়ভাবে ফিল্টার করে বাদ দেওয়া হয়।
2. **কম্পাউন্ড সাবস্ট্রিং সংঘর্ষ রোধ (`COMPOUND_COLLISIONS`):**
   - `elitebrosauto@...` থেকে ভুলবশত "Eli" হওয়া রোধ।
   - `anaheimauto@...` থেকে শহরের নাম "Ana" হওয়া রোধ।
   - `tomballaseauto@...` থেকে শহরের নাম "Tom" হওয়া রোধ।
   - `georgetownmuffler@...` থেকে শহরের নাম "George" হওয়া রোধ।
3. **শহরের নাম ফিল্টার (`CITY_NAME_COLLISIONS`):**
   - Tracy, Carson, Davis, Roseville, Bell, Campbell, Dixon, Selma, Austin, Tyler ইত্যাদি শহরের নাম কোম্পানিতে থাকলে সেগুলোকে মানুষের নাম ধরা সম্পূর্ণ বন্ধ।
4. **কর্পোরেট চেইন ফিল্টার:**
   - Caliber Collision, Valvoline, Firestone, Meineke, Chevron, Texaco ইত্যাদি ন্যাশনাল চেইন বাদ।

---

## 🎛️ ফুল-কন্ট্রোল ড্যাশবোর্ড ও মেনু (Full Control Dashboard)

IP ব্লক এড়াতে এবং সম্পূর্ণ নিয়ন্ত্রণ আপনার হাতে রাখতে ইন্টারেক্টিভ কন্ট্রোল ড্যাশবোর্ড যোগ করা হয়েছে।

টার্মিনালে গিয়ে রান করুন:
```bash
cd /home/tuhin/Documents/Auto
python3 run_pipeline.py
```

### কন্ট্রোল মেনুর অপশনসমূহ:
- **[1] 🛡️ Run 1 Batch (৩টি সার্চ):** মাত্র ১টি ব্যাচ রান করে স্বয়ংক্রিয়ভাবে থেমে যাবে (IP সেফটির জন্য সেরা)।
- **[2] 🔢 Run Custom Queries:** নির্দিষ্ট সংখ্যক সার্চ রান করতে (যেমন: ১, ২, ৫টি)।
- **[3] ⚡ Run Enrichment ONLY:** গুগল ম্যাপে হাত না দিয়ে আগের স্ক্র্যাপ করা শপগুলো থেকে নতুন ইমেইল এক্সট্রাক্ট ও ভেরিফাই করতে।
- **[4] 🚀 Continuous Auto-Run:** প্রতি ৩টি সার্চ শেষে ৬০ সেকেন্ড (১ মিনিট) বিরতি দিয়ে স্বয়ংক্রিয়ভাবে চলতে থাকবে।
- **[5] 📊 Refresh Status:** বর্তমান অগ্রগতি ও স্ট্যাটাস রিফ্রেশ করে দেখাবে।
- **[0] 🛑 Exit:** প্রগ্রেস সেভ রেখে যেকোনো সময় নিরাপদে বের হয়ে যাওয়া।

---

## ⚡ কুইক কমান্ড শর্টকাট (CLI Flags)

```bash
# ১. বর্তমান অগ্রগতি ও স্ট্যাটাস দেখতে
python3 run_pipeline.py --status

# ২. মাত্র ১টি ব্যাচ (৩টি সার্চ) রান করে থেমে যেতে
python3 run_pipeline.py --batch 1

# ৩. ব্যাকগ্রাউন্ডে স্বয়ংক্রিয়ভাবে পুরো পাইপলাইন রান করে রাখতে (nohup)
nohup python3 run_pipeline.py > pipeline.log 2>&1 &

# ৪. ব্যাকগ্রাউন্ডের লাইভ লগ রিয়েল-টাইমে দেখতে
tail -f pipeline.log

# ৫. গুগল ম্যাপ ছাড়া শুধু ইমেইল কালেকশন ও ভেরিফিকেশন চালাতে
python3 run_pipeline.py --enrich-only
```

---

## 📁 পাইপলাইনের মূল ফাইলসমূহ

| ফাইল | ভূমিকা | আউটপুট |
|---|---|---|
| [run_pipeline.py](file:///home/tuhin/Documents/Auto/run_pipeline.py) | মাস্টার অর্কেস্ট্রেটর ও কন্ট্রোল ড্যাশবোর্ড | কনসোল ইন্টারফেস |
| [queries.txt](file:///home/tuhin/Documents/Auto/queries.txt) | ৭৬০টি ক্যালিফোর্নিয়া সিটি ও কি-ওয়ার্ড তালিকা | ইনপুট কোয়ারি |
| [completed_queries.txt](file:///home/tuhin/Documents/Auto/completed_queries.txt) | সম্পন্ন হওয়া সার্চের লগ (রেজুমের জন্য) | ট্র্যাকিং ফাইল |
| [gmaps_scraper.py](file:///home/tuhin/Documents/Auto/gmaps_scraper.py) | ধাপ ১: ডিপ-স্ক্রলিং গুগল ম্যাপস স্ক্র্যাপার | `shops.csv` & `no-website.csv` |
| [no-website.csv](file:///home/tuhin/Documents/Auto/no-website.csv) | ওয়েবসাইটবিহীন শপ (ওয়েব ডেভেলপমেন্ট সার্ভিস কোল্ড কলিংয়ের জন্য) | কোল্ড কলিং লিড |
| [email_spider.py](file:///home/tuhin/Documents/Auto/email_spider.py) | ধাপ ২: ফিল্টারযুক্ত Scrapy ইমেইল এক্সট্রাক্টর | `shops_with_emails.csv` |
| [verify_emails.py](file:///home/tuhin/Documents/Auto/verify_emails.py) | ধাপ ৩: ফ্রি DNS ও MX রেকর্ড ইমেইল ভেরিফায়ার | `verified_shops.csv` |
| [clean_leads.py](file:///home/tuhin/Documents/Auto/clean_leads.py) | ধাপ ৪: v2.2 আল্ট্রা-ক্লিন লিড ফরম্যাটার | `campaign_ready_leads.csv` |
| [generate_ca_queries.py](file:///home/tuhin/Documents/Auto/generate_ca_queries.py) | ৭৬০টি ক্যালিফোর্নিয়া সার্চ কোয়ারি জেনারেটর | `queries.txt` |
| [campaign_ready_leads.csv](file:///home/tuhin/Documents/Auto/campaign_ready_leads.csv) | চূড়ান্ত ভেরিফাইড ও ১০০% ক্লিন লিড তালিকা | প্রস্তুত কোল্ড ইমেইল আউটপুট |
| [tx_archive/](file:///home/tuhin/Documents/Auto/tx_archive/) | টেক্সাস (TX) ক্যাম্পেইনের সংরক্ষিত ফাইলসমূহ | ব্যাকআপ ডিরেক্টরি |
