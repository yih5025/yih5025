# Ilhan Yu - Data Engineer

## Projects

### 1. **WeInvesting - Real-Time Investment Intelligence Platform**

**Description**: A comprehensive real-time financial information platform that integrates multiple data sources to provide investors with market data, cryptocurrency prices, economic indicators, news, and social sentiment analysis—all in one unified dashboard.

**Tech Stack**:

<img src="https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=apacheairflow&logoColor=white"/> <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white"/> <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"/> <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white"/> <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/> <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white"/> <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white"/> <img src="https://img.shields.io/badge/WebSocket-010101?style=for-the-badge&logo=socketdotio&logoColor=white"/>

**Key Features**:
- **Zero-cost data collection** using 7 free-tier APIs (NewsAPI, Finnhub, Alpha Vantage, FRED, etc.)
- Real-time data streaming via **WebSocket** with Redis Pub/Sub architecture
- Comprehensive coverage: **S&P 500 stocks**, **415+ cryptocurrencies**, **ETFs**, and **economic indicators**
- **30 database tables** across 4 domains (stocks, crypto, news, economic data)
- AI-powered **social sentiment analysis** from X/Twitter and Truth Social
- **5-node K3s cluster** for distributed data pipeline execution
- **13,115 lines** of orchestrated DAG code managing automated data collection

---

#### 📊 **Data Collection (Apache Airflow)**

Built a robust data pipeline using **Apache Airflow** to orchestrate automated collection from multiple financial APIs into a PostgreSQL database.

**Infrastructure & Scale**:
- **5-node K3s cluster** running Apache Airflow 2.8
- **35 Python DAG files** and **60 SQL scripts**
- **30 database tables** organized across 4 primary domains
- Daily throughput of **1,000+ data points**

**Data Domains**:
1. **Stock Data** (9 tables): Company overviews, balance sheets, earnings calendars, market news
2. **Cryptocurrency** (9 tables): Real-time pricing from Bithumb and CoinGecko with cross-platform mapping
3. **News & Sentiment** (8 tables): Market news, social media posts with sentiment scores
4. **Economic Indicators** (4 tables): CPI, inflation rates, federal funds rate, treasury yields

**API Rate Limit Optimization**:
- Implemented dual-token rotation with 15-minute delays for X API
- Progressive batching strategy (20 symbols daily) with checkpoint-based resumption
- Real-time rate limit detection with automatic 30-second backoff
- Weekday-specialized queries increasing efficiency by **3x**

**Scheduling Model**:
- **Hourly**: 5 real-time data collection DAGs
- **Daily**: 15 batch processing DAGs (03:00 UTC)
- **Weekly**: 8 DAGs (Sundays, 05:00 UTC)
- **Monthly**: 7 DAGs (1st of month)

**Achievement**: Built an enterprise-grade data pipeline with **$0 infrastructure cost** by leveraging free-tier APIs and optimized rate limit handling.

#### 🖥️ **Airflow Web UI**
<img src="./assets/weinvesting-airflow.png" alt="WeInvesting Airflow Web UI" width="1000"/>

---

#### ⚙️ **Backend (FastAPI)**

Developed a high-performance **FastAPI-based RESTful API server** that aggregates and serves real-time financial data through **24+ endpoints**.

**Tech Stack**:
- **FastAPI** v0.104.1 with async/await architecture
- **PostgreSQL** with 27+ ORM models and optimized indexing
- **Redis** v5.0.1 for caching and Pub/Sub messaging
- **WebSockets** v13.0.1 for bidirectional real-time streaming
- **Kubernetes/K3s** deployment in `investment-assistant` namespace

**Real-Time Streaming Architecture**:
- **5 WebSocket channels**: crypto prices, S&P 500 updates, market data, ETF tracking
- **Redis Pub/Sub** for server-initiated data broadcasting
- Automatic dead connection cleanup with heartbeat monitoring

**API Categories** (All under `/api/v1` prefix):
- **Market News**: 6 endpoints with search, filtering, and statistics
- **S&P 500**: Market overview, symbol details, interactive charts
- **Cryptocurrency**: Real-time pricing for 415+ coins, investment analysis, ecosystem metrics
- **Economic Data**: CPI, Federal Funds Rate, Treasury Yields with historical trends
- **Social Sentiment**: X/Twitter and Truth Social posts with sentiment analysis

**Performance Optimizations**:
- Two-layer Redis caching with 60-second TTL
- Compound database indexes on symbols and timestamps
- Batch query operations reducing database round-trips
- Async I/O throughout the entire stack
- Connection pooling with SQLAlchemy v2.0.23

**Project Scale**:
- **21,369+ lines** of Python code
- **27+ ORM models** with relationship mapping
- **415+ cryptocurrencies** supported
- **Live API**: https://api.investment-assistant.site

---

#### 💻 **Frontend (React + TypeScript)**

Built a modern, responsive web application providing an intuitive dashboard for real-time investment intelligence.

**Tech Stack**:
- **React 18** with **TypeScript** and **Vite** build tool
- **Tailwind CSS** and **Radix UI** for modern, accessible design
- **Recharts** and **Lightweight Charts** for financial data visualization
- **React Query** for server state and **Zustand** for client state management
- **Socket.IO Client** for WebSocket real-time updates
- **Framer Motion** for smooth animations

**Five Primary Modules**:
1. **Home Dashboard**: Real-time market overview, earnings calendar, IPO tracker
2. **Market Intelligence**: Live stock (S&P 500, NASDAQ), crypto (Bitcoin, Ethereum), and ETF data with technical charts
3. **SNS Analytics**: X/Twitter post analysis with AI-powered investor sentiment detection
4. **Financial News**: Real-time news feed with summaries and ticker connections
5. **Economic Indicators**: Macroeconomic dashboard tracking rates, inflation, GDP with historical trends

**Key Features**:
- Mobile-first responsive design (optimized for 320px–768px)
- Dark mode support with system preference detection
- Real-time data updates via WebSocket streams
- Performance optimizations: lazy loading, memoization, code splitting
- Browser history management with state restoration
- XSS prevention and environment-based security

**Live Platform**: https://weinvesting.site

#### 📱 **App Screenshots**
<table>
  <tr>
    <td><img src="./assets/weinvesting-app-1.png" alt="WeInvesting Home Dashboard" width="100%"></td>
    <td><img src="./assets/weinvesting-app-2.png" alt="WeInvesting Market Intelligence" width="100%"></td>
    <td><img src="./assets/weinvesting-app-4.png" alt="WeInvesting SNS Analytics" width="100%"></td>
  </tr>
  <tr>
    <td align="center">홈 페이지 화면 (실적발표 캘린더)</td>
    <td align="center">암호화폐 상세보기 페이지 화면</td>
    <td align="center">트럼프 sns 분석 페이지 화면</td>
  </tr>
</table>

---

#### 🔗 **GitHub Repositories:**
[Data Collection (DAGs)](https://github.com/yih5025/investment-assistant-dags) | [Backend API](https://github.com/yih5025/investment-assistant-backend) | [Frontend](https://github.com/yih5025/investment-assistant-frontend)

#### 👨🏻‍🔧 **Data Pipeline Architecture**
<img src="./assets/weinvesting-pipeline.jpg" alt="WeInvesting Data Pipeline Architecture" width="1000"/>

#### 🏗️ **Infrastructure Architecture**
<img src="./assets/weinvesting-infrastructure.jpg" alt="WeInvesting Infrastructure Architecture" width="1000"/>

---

### 2. **Cryptocurrency Mock Investment Game**

**Description**: A web-based mock investment game that uses real-time cryptocurrency data for fun and educational investing.

**Tech Stack**:

<img src="https://img.shields.io/badge/Apache%20Kafka-000?style=for-the-badge&logo=apachekafka&logoColor=white"/> <img src="https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white"/> <img src="https://img.shields.io/badge/AWS%20Lambda-FF9900?style=for-the-badge&logo=awslambda&logoColor=white"/> <img src="https://img.shields.io/badge/AWS%20S3-orange?style=for-the-badge&logo=amazons3&logoColor=white"/> <img src="https://img.shields.io/badge/Amazon%20Kinesis-8b4fff?style=for-the-badge&logo=amazonaws&logoColor=white"/> <img src="https://img.shields.io/badge/AWS%20API%20Gateway-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white"/> <img src="https://img.shields.io/badge/AWS%20DynamoDB-4053D6?style=for-the-badge&logo=amazon-dynamodb&logoColor=white"/>

**Key Features**:
- Real-time data ingestion of up to **900,000 records per minute**
- Automated pipeline for data collection
- **Serverless backend** to reduce costs and simplify management
- To address cloud service cost issues, I replaced AWS EMR with an on-premises distributed computing environment, reducing server costs by **94% ($140 → $7 per month)**
- Collected clickstream data to derive user data analysis

#### 🎥 **Game Demo Video: [Game Video](https://youtu.be/REJ2J8oUgwQ?si=xgPsktEdx1rFc1Sq)**
#### 🔗 **GitHub Repository:** [Organization DE Repo](https://github.com/SCHU-CoinGame/DataEngineering), [Organization BE Repo](https://github.com/SCHU-CoinGame/BackEnd)
#### 🏆 **Award: Game Development Contest top prize**
#### **More Details:** [Tech Blog Post](https://dont-make-excuses.tistory.com/18)
### 👨🏻‍🔧 **Designed Data Pipe Line Architecture**
<img src="./assets/pipeline.jpg" alt="CoinGame DataPipeline Architecture" width="1000"/>

### 📈 **In-Game Screenshot**
<img src="./assets/ingame.png" alt="CoinGame In-Game Screenshot" width="1000"/> 
<img src="./assets/coingame1.png" alt="CoinGame In-Game Screenshot" width="1000"/>

---

### 2. **Large-Scale IoT Data Analysis Paper**

#### **Title**: "사물인터넷 환경 저비용 대용량 데이터 분석 시나리오 설계 및 성능 비교"  
#### **Conference**: KCC2024 *(한국컴퓨터종합학술대회)*
#### 🏆 **Award: Best Student Paper Award at KCC2024**

**Tech Stack**: 
<img src="https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Hadoop-66CCFF?style=for-the-badge&logo=apachehadoop&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Zeppelin-2A3C54?style=for-the-badge&logo=apache&logoColor=white" />
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" />

**Key Highlights**:
- Built a **low-cost Raspberry Pi cluster** using Hadoop (HDFS, Yarn) and Spark (DataFrame)
- Collected and analyzed **22-year particulate matter data** from AirKorea
- Compared performance of Spark and Pandas on **Raspberry Pi cluster vs. a standard PC**
- Achieved **Best Student Paper** at the Korean Computer Congress 2024

#### **Paper Link:** [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE11862340)
#### **More Details:** [Tech Blog - Paper Category](https://dont-make-excuses.tistory.com/category/%EB%85%BC%EB%AC%B8)

### **Low-Cost IoT Device Based Cluster Setup**
<img src="./assets/Picture3.png" width="600">

### **Results:**

I computed the time that took to analyze certain size of data in four different settings. The settings are:
+ Spark on single RPi 
+ Spark on RPi cluster
+ Spark on a normal desktop PC
+ Pandas on a normal desktop PC

<img src="./assets/Picture1.png" width="600">

> Single RPi could complete the task of analyzing big data, though pandas on a normal desktop PC failed due to OOM. Spark on a normal desktop PC showed the best performance, followed by Spark on RPi cluster, then Spark on single RPi.

<img src="./assets/Picture2.png" width="600">

> So then I used less amount of data for experiment - Pandas on a normal desktop PC showed best performance, followed by Spark on a normal desktop PC, then Spark on a RPi cluster, and then Spark on single RPi.

<img src="./assets/papar_ML_2.png" width="600">

> I tried linear regression to predict PM using Scikit-learn and Spark ML. Similar to the experiment result above, when it comes to big data, Pandas and Scikit-learn failed due to OOM, and Spark on RPi cluster succesfully completed machine learning as well as data analysis.

### **Conclusion**

+ Spark on RPi cluster showed fine performance based on relatively low cost
+ Resolved the necessity of costly servers in IoT environments for big data analysis and machine learning

### **KCC2024 Poster**
<img src="./assets/poster.png" width=600>

---

### 3. **The Korean air pollution data Visualization**

**Description:** A web-based application that collects and visualizes air pollution data from Korea’s public API (such as AirKorea). The site displays monthly trends in pollutants—including PM10, PM2.5, SO₂, NO₂, CO, and O₃—and provides users with the latest air quality indices for various regions. This helps users easily compare regional air quality and understand historical pollution patterns.

**Tech Stack**: <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=java&logoColor=white" alt="Java" /> <img src="https://img.shields.io/badge/Spring-6DB33F?style=for-the-badge&logo=spring&logoColor=white" alt="Spring" /> <img src="https://img.shields.io/badge/InfluxDB-000?style=for-the-badge&logo=influxdb&logoColor=white" alt="InfluxDB" /> <img src="https://img.shields.io/badge/Grafana-F26A21?style=for-the-badge&logo=grafana&logoColor=white" alt="Grafana" /> <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5" /> <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3" /> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />

#### **Key Features** 
- **Visualization:** Graphical display of monthly air pollution trends for various regions.
- **Real-Time Data Updates:** Fetches and updates current PM10, PM2.5, and other pollutant data.
- **Regional Comparison:** Compare air quality indices across multiple regions in Korea.
- **Historical Data Analytics:** Analyze past pollution trends and patterns.
- **Seaching Regional Data:** Loads data into MYSQL and displays only data for a specific region in a table format.

#### 🔗 **GitHub Repository:** [Korea Air Pollution Repo](https://github.com/yih5025/Korea-Air-Pollution)

### 💨 **WebPage Screenshot**
![Air Pollution Image 1](https://raw.githubusercontent.com/yih5025/Korea-Air-Pollution/main/assets/image1.png)
![Air Pollution Image 2](https://raw.githubusercontent.com/yih5025/Korea-Air-Pollution/main/assets/image2.png)

---
### 4. **Assistive Device for the Visually Impaired Using NFC, OCR and Object Detection(Voice Memo.)**

**Description:** Voice Memo. is an assistive technology platform designed to help visually impaired individuals easily access information about objects. By leveraging object detection, OCR, and voice recording, the application allows users to capture information about items (via NFC stickers) and listen to audio memos detailing object-specific information.

### 🎥 **Demo Video: [Demo Video](https://youtu.be/uVzv5m8E1PE?si=lztpBMtm8oQlKLba)**

**Tech Stack:** <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=java&logoColor=white" alt="Java" /> <img src="https://img.shields.io/badge/Spring-6DB33F?style=for-the-badge&logo=spring&logoColor=white" alt="Spring" /> <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white" alt="Flutter" /> <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" /> <img src="https://img.shields.io/badge/Google%20ML-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Google ML" /> <img src="https://img.shields.io/badge/Fusion360-007ACC?style=for-the-badge&logo=autodesk&logoColor=white" alt="Fusion360" />

#### **Key Features** 
- **Google ML API:** for object detection and OCR  
- **Raspberry Pi & NFC Modules:** for IoT-based voice memo storage  
- **TCP Socket Communication:** for data transfer between devices

#### 🔗 **More Details:** [Tech Blog - VoiceMemo](https://dont-make-excuses.tistory.com/9)

### **Service Architecture**
<img src="./assets/voicememo9.jpg" width="800"/>

### 📱 **App Feature Description**
<img src="./assets/voicememo3.jpg" width="370"/> <img src="./assets/voicememo4.jpg" width="400"/>

---

## 💬 About Me
- I’m a **Data Engineer** with experience building real-time data pipelines using **Kafka**, **Spark**, and **Hadoop**.
- I also developed backend services, including **Spring**-based systems and **AWS Lambda/API Gateway**, to ensure scalability and reliability.
- On the frontend side, I’m comfortable with **JavaScript**, **HTML**, and **CSS**.
- For database solutions, I’ve worked with **DynamoDB**, **InfluxDB**, and **MySQL**, focusing on efficient data modeling and queries.
  
**Tech Focus**:
- **Data Engineering**: Kafka, Spark, Hadoop, Designed ELT/ETL Data Pipeline
- **Data Analysis**: Spark DataFrame, Pandas
- **Backend**: Spring, AWS Lambda, API Gateway
- **Frontend**: JavaScript, HTML, CSS
- **Databases**: DynamoDB, InfluxDB, MySQL
---

## 🌱 Mission
> "I aim to use diverse data sources to build practical services that truly solve real-world problems and help people."

Currently, I want to develop a platform that makes it easy for anyone to collect and handle complex data, bridging the gap between raw information and real-life solutions. I hope to focus on accessible data engineering that benefits people’s daily needs without being overly complicated.

---

## 🏆 Achievements
- **Game Development Contest Winner**: Developed a real-time cryptocurrency mock investment game, earning top prize.
- **App Development Contest Winner**: Developed a "1 Dollor Breakfast" users monitoring app, earning top prize.
- **SW Idea Contest Winner**: Suggested the idea of AI special agreement suggestion service for "Jeonse" contracts, earning second prize.
- **Best Student Paper Award at KCC2024**: Authored a paper on low-cost IoT-based big data analysis with Raspberry Pi clusters, Spark & Hadoop.
---

## 🤝 Contact 
- **LinkedIn**: [linkedin.com/in/ilhan-yu](https://www.linkedin.com/in/ilhan-yu-8b3a6b2b9/)
- **Email**: [yih5025@gmail.com](mailto:yih5025@gmail.com)
- **Tech Blog**: [dont-make-excuses](https://dont-make-excuses.tistory.com/)
