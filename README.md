# Yemek Tarifleri RAG Chatbotu

Yemek Tarifleri RAG Chatbotu, Türk mutfağına ait tarifleri içeren geniş bir veri seti üzerinde, kullanıcıların doğal dille soru sorarak anlık ve doğru cevaplar almasını sağlayan, **Retrieval Augmented Generation (RAG)** mimarisine sahip bir yapay zeka uygulamasıdır.

Bu proje, Akbank GenAI Bootcamp kapsamında geliştirilmiştir ve dil modellerinin bilgi tabanını genişletme, halüsinasyonları azaltma ve spesifik bir alana odaklama zorluklarını çözmeyi amaçlamaktadır.

## Web Arayüzü & Product Kılavuzu

Deploy Link: (https://meningeal-darin-unreprobatively.ngrok-free.dev/)

Projenin Detaylı Çalışma Anlatımı (Video Kılavuz):

[▶️ Proje Demoyu İzlemek İçin Tıklayın (YouTube)] https://www.youtube.com/watch?v=CwBejnoAseY

Çalışma Akışı:

Kullanıcı, yukarıdaki adresi ziyaret ettiğinde chatbot arayüzü ile karşılaşır.

Sol üstte "Yemek Tarifleri RAG Chatbotu" başlığını görür.

Chatbot, bir hoş geldin mesajı ile kullanıcıyı karşılar.

Kullanıcı, alt kısımdaki metin kutusuna bir tarif sorusu yazar (Örn: "sodalı köfte tarifi").

Soru gönderildiğinde RAG zinciri devreye girer:

Soru, vektör veritabanında en alakalı 6 tarif parçasını çeker.

Çekilen bağlam ve soru, Gemini 2.5 Flash'a gönderilir.

LLM, aldığı sistem talimatına uygun olarak, sadece sağlanan bağlamı kullanarak ve belirtilen formatta (Yemek Adı, Malzemeler, Yapılışı) cevabı üretir.

Cevap, kullanıcıya sohbet baloncuğu içinde gösterilir.

Eğer kullanıcı veri setinde olmayan bir şey sorarsa (Örn: "Amerikan hot-dog tarifi nedir?"), LLM sadece şu yanıtı döndürür: "Üzgünüm, aradığınız tarifi şu anda veri tabanımda bulamıyorum. Belki başka bir yemek tarifi sormak istersiniz?"


## Temel Özellikler

* **RAG Mimarisi:** Doğru ve bağlama uygun tarifler sunmak için LangChain ve Gemini API'si ile tam bir RAG zinciri uygulanmıştır.
* **Veri Odaklı Cevaplama:** Model, yalnızca kendisine sağlanan tarif (BAĞLAM) üzerinden cevap vermek üzere katı bir şekilde yönlendirilmiştir. Veri setinde bulunmayan soruları reddetme yeteneğine sahiptir.
* **Standartlaştırılmış Çıktı:** Cevaplar her zaman **Yemek Adı**, **Malzemeler** ve **Yapılışı** başlıkları altında düzenlenmiş, temiz bir formatta sunulur.
* **Kalıcı Vektör Veritabanı:** Tarama hızını artırmak için ChromaDB kullanılarak veri seti kalıcı olarak gömülmüştür.
* **Web Arayüzü:** Uygulama, Streamlit üzerinde modern ve kullanımı kolay bir sohbet arayüzü ile sunulmaktadır.

## Kullanılan Teknolojiler

| Kategori | Teknoloji / Model | Açıklama |
| :--- | :--- | :--- |
| **Büyük Dil Modeli (LLM)** | Google Gemini 2.5 Flash | Yüksek hızda, düşük gecikmeli cevap üretimi için kullanıldı. |
| **Embedding Modeli** | Google `text-embedding-004` | Metin parçalarını vektörlere dönüştürmek için kullanıldı. |
| **RAG Çerçevesi** | LangChain | RAG zincirini (Retriever, Prompt, LLM) kurmak ve yönetmek için kullanıldı. |
| **Vektör Veritabanı** | ChromaDB | Vektörlerin depolanması ve hızlı aranması için kullanıldı. |
| **Web Arayüzü** | Streamlit | Chatbot için interaktif ve kolay bir arayüz oluşturmak için kullanıldı. |
| **Veri Seti** | `mertbozkurt/llama2-TR-recipe` (Hugging Face) | Türk mutfağına ait yemek tariflerini içerir. |

## Veri Seti Hakkında

Bu projede, Hugging Face platformunda bulunan **`mertbozkurt/llama2-TR-recipe`** veri seti kullanılmıştır.

* **İçerik:** Veri seti, ağırlıklı olarak Türk mutfağına ait yemek tariflerini içeren metinlerden oluşmaktadır.
* **Hazırlık Metodolojisi:** Veri setinin ilk 5000 tarifi (kayıt) alınmıştır. Her tarif belgesi (Document) oluşturulurken, tarifi oluşturan orijinal etiketler (`<s>[INST]`, `[/INST]`, `</s>`) temizlenmiş ve metin içeriği sadeleştirilmiştir.
* **Parçalama:** Temizlenen belgeler, `RecursiveCharacterTextSplitter` kullanılarak `4000` karakter büyüklüğünde ve `200` karakter çakışmalı parçalara (chunks) ayrılmıştır.

## Yerel Kurulum ve Çalıştırma Kılavuzu

Projeyi kendi ortamınızda çalıştırmak için aşağıdaki adımları takip edin.

### Ön Gereksinimler

* Python (önerilen 3.10+)
* Git
* Google Gemini API Anahtarı

### Adım 1: Depoyu Klonlama

```bash
git clone (https://github.com/dogakusun/Yemek_Tarifleri_RAG_Chatbotu)
cd (Yemek_Tarifleri_RAG_Chatbotu)

### Adım 2: Sanal Ortam Oluşturma ve Gerekli Kütüphaneleri Kurma

# Sanal ortam oluşturma
python -m venv venv

# Sanal ortamı etkinleştirme (Linux/Mac)
source venv/bin/activate
# Sanal ortamı etkinleştirme (Windows)
# venv\Scripts\activate

# Gerekli kütüphaneleri kurma
pip install -q google-genai langchain langchain-community chromadb streamlit datasets pypdf langchain-google-genai

###Adım 3: API Anahtarını Ayarlama

GEMINI_API_KEY anahtarınızı ortam değişkeni olarak ayarlayın.

# Linux/Mac
export GEMINI_API_KEY='YOUR_API_KEY_HERE'

# Windows (Komut İstemi)
set GEMINI_API_KEY='YOUR_API_KEY_HERE'

###Adım 4: Veritabanını Oluşturma (İlk Çalıştırma)

Bu adım, chroma_db_tarifler dizinini ve vektör veritabanını oluşturur.

Önemli: Projenin Colab'de oluşturulan kısmı, veritabanını oluşturma (Embedding) ve RAG zincirini kurma adımlarını içerir. Bu adımları yerel ortamınızda çalıştırmak için Colab'deki ilgili kod bloklarını (2.1, 2.2, 2.3, 3.1, 3.2) bir Python dosyasına (örneğin setup_db.py) taşıyarak çalıştırmanız veya app.py dosyasındaki @st.cache_resource fonksiyonunu yerel olarak veritabanını oluşturacak şekilde düzenlemeniz gerekir.

Yerel Ortam İçin Basitleştirilmiş Çalıştırma: Eğer Colab'de ./chroma_db_tarifler klasörünü sıkıştırıp (zip) indirdiyseniz ve projenizin ana dizinine çıkardıysanız, bu adımı atlayabilirsiniz. Aksi takdirde, Colab'deki 2. adımı (Veri Seti Hazırlama, Parçalama ve ChromaDB Oluşturma) çalıştıracak bir setup_db.py dosyası oluşturup bir kere çalıştırmalısınız.

###Adım 5: Streamlit Uygulamasını Başlatma

streamlit run app.py
Uygulama, tarayıcınızda http://localhost:8501 adresinde açılacaktır.

#İletişim
Proje ile ilgili sorularınız için iletişime geçebilirsiniz.

GitHub: github.com/dogakusun
