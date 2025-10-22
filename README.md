# Yemek Tarifleri RAG Chatbotu

Yemek Tarifleri RAG Chatbotu, Türk mutfağına ait tarifleri içeren geniş bir veri seti üzerinde, kullanıcıların doğal dille soru sorarak anlık ve doğru cevaplar almasını sağlayan, **Retrieval Augmented Generation (RAG)** mimarisine sahip bir yapay zeka uygulamasıdır.

Bu proje, Akbank GenAI Bootcamp kapsamında geliştirilmiştir ve dil modellerinin bilgi tabanını genişletme, halüsinasyonları azaltma ve spesifik bir alana odaklama zorluklarını çözmeyi amaçlamaktadır.

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

