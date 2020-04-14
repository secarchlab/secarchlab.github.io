# SecArch Lab

兵庫県立大学 大学院応用情報科学研究科 セキュアアーキテクチャ研究室

---

## News

- Apr. 2020: Our paper on private information retrieval was accepted by IEICE Communications Express (ComEx). PIRについての原稿がIEICE ComExに採録されました。
  
  > Jun Kurihara and Toru Nakamura, ``On the Resistance to Byzantine and Unresponsive Servers in Code-based PIR Schemes,'' to appear in *IEICE Communication Express*, 2020.

- Mar. 2020: Uploaded slides for introduction to this lab for students. 学生の方々向けに、研究室の紹介スライドをアップしました。 [[Link to PDF](./repo/lab-info-20200326.pdf)]
- Feb. 2020: Published a survey article "Network Operations as an Infrastructure for Diverse Businesses" coauthored by Jun at Journal of IEICE. 電子情報通信学会誌に、栗原が共著のサーベイ記事「多様なビジネスの収容基盤としてのネットワークとその課題──著作権保護や消費税徴収等に関わる技術的・法的課題──」が掲載されました。[[Link to summary](https://www.journal.ieice.org/summary.php?id=k103_2_155&year=2020&lang=J)]
- Jan. 2020: SecArch Lab. just started! 研究室を立ち上げました！

---

## Research 研究内容

- Information security and privacy mechanisms using coding theoretic techniques, e.g., secret sharing, private information retrieval, etc.
  
  情報理論や符号理論の技術を用いた、セキュリティ・プライバシ保護手法の研究。例えば、秘密分散法、private information retrievalなど。
  
  - Strongly-secure universal secure network coding (IEEE Trans. Inf. Theory, 2015): 強い安全性を有するユニバーサルネットワーク符号化の構成手法と安全性評価の研究
  - Relationship between parameters of linear codes and security of secret sharing scheme (IEICE Trans., 2013): 線形符号のパラメータと秘密分散法の安全性との関連性を解明する研究
  - Exclusive-OR-based high speed secret sharing (IEICE Trans., 2009): 超高速な秘密分散法のアルゴリズム構成と、その安全性証明手法の研究
  - etc

- Information-centric networking (Named-data networking; NDN) and its brand-new security mechanisms, e.g., security in edge named function environment.

  情報指向ネットワークにおける、新しいセキュリティメカニズムの研究。
  
  - Access-control-based censorship circumvention in information-centric networking (ACM ICN, 2016): コンテンツの暗号化ベースのアクセス制御を応用した、情報指向ネットワークでの検閲回避方式の構成と評価の研究
  - List-based interest enabling high speed forwarding in information-centric networking (IEICE Trans., 2016): 情報指向ネットワークのリクエストパケットのリスト化によるルータ転送高速化手法の構成、理論的、実装的評価の研究
  - etc.

- In-network and edge computing architecture, its security and privacy, e.g., new theoretical (and mathematical) framework of secure communication and computation, privacy-preserving computation at untrusted third-party edge environment.

  ネットワーク内コンピューティング・エッジコンピューティングなど、新しい計算アーキテクチャにおけるセキュリティやプライバシの数理的フレームワークの研究や、信頼できない計算基盤でのユーザプライバシ保護方法の研究
  
  - Under construction... 構成中
  

For detailed research topics and publication, please check [Jun's web site](https://junkurihara.github.io). Also for students, please check [[the slides for introduction to this lab (PDF)](./repo/lab-info-20200326.pdf)]

詳細な研究内容や出版履歴については、[栗原のWebサイト](https://junkurihara.github.io) を参照して下さい。
学生の方向けの[[研究室紹介資料 (PDF)](./repo/lab-info-20200326.pdf)]もご参照下さい。

---

## Member メンバー

### Faculty 教員

- Jun KURIHARA / 栗原 淳 [[WebSite](https://junkurihara.github.io)]

### Students 学生

- (From next fiscal year; 次年度から)

---

## Information to students. 学生の方々へ

※Introduction to this lab. 研究室紹介資料 [[Link to PDF](./repo/lab-info-20200326.pdf)]

### To choose your supervisor, and to start your research activity. 研究室選び、そして研究を始めるにあたって

**We strongly recommend you to read a great guide 'how to research' written by Prof. Tomohiko Uyematsu at Tokyo Tech**. All fundamental elements for research are described in the guide. [[link](http://www.it.ce.titech.ac.jp/uyematsu/howtoresearch.pdf)] [[mirror](./repo/howtoresearch.pdf)] (Copyright Prof. Uyematsu)

栗原の師匠である**東京工業大学 植松友彦先生の書かれた「研究読本」には必ず目を通しましょう**。 研究室を選んで、そして研究を始めるにあたって、必要な事柄が全て記載されています。 [[link](http://www.it.ce.titech.ac.jp/uyematsu/howtoresearch.pdf)] [[mirror](./repo/howtoresearch.pdf)] (Copyright Prof. Uyematsu)

**If you wish to research in this lab, please contact Jun Kurihara before applying the admission exam.** I believe that we should have a chat (F2F or online) beforehand in order to have good research topics at this lab and avoid any misunderstanding on our research.

**もしこの研究室で研究したいと思ったら、入試を受ける前にまず栗原にコンタクトを取りましょう**。より良い研究を行うためにも、この研究室での研究に対する誤解を避けるためにも、まずはぜひお話ししましょう！F2Fでもオンラインでも対応します。

---

### Research style and tools in our lab. この研究室の研究スタイルとか

Our research mostly uses some of following tools, and (possibly) highly requires a certain of their background skills and knowledge. Anyways, I believe that the most important thing is the motivation to 'continuously' study these tools. (Even I need to study hard and hard for nicer research!)

この研究室での研究は、以下のような手法やツールを使って行います。そのため、ある程度のこれらの知識やスキルも重要になってきます。いずれにしろ、これらを継続して習熟しようとするモチベーションが一番重要だと考えます。※栗原ももっと良い研究をするにはまだまだ勉強が足りません！

- First of all, **English**. Cutting edge papers are always written and published in English. We mostly refer to English litertures even textbooks.

  何はともあれ、**英語**。最新の論文は常に英語で書かれ、出版されます。ほとんどの場合、教科書ですら、英語の文献を参照します。

- **Mathematics**, especially 'linear algebra', 'group/ring/field theory', 'probability theory', etc. We use mathematics everywhere to define the security problem, build the solution to the problem, and prove the security of the solution. 

  **数論**。特に、線形代数、群環体論、確率論、他。これらはセキュリティ課題を定義し、その課題に対する解決策を構築し、その解決策の安全性を証明するため、あらゆるところで使います。
  
- Basics of coding theory, information theory, computational theory and cryptography in order to build the basic component of security-related networking and computational architecture.

  セキュリティを考慮したネットワーク・計算アーキテクチャを構成するための、符号理論や情報理論、計算理論、暗号理論の基礎。
  
- Knowledge on 'networking architecture' including the architecture of the Internet, and 'system of digital communication'. e.g., TCP/IP, DNS, Wi-Fi system, etc.

  インターネットの構成を含む「ネットワーク構造」や「デジタル通信システム」、また「計算機アーキテクチャ」についての知識。例えばTCP/IP、DNSやWi-Fiシステム、OSや仮想化技術、等。

---

### Environments to research 研究するための環境とか

- Unix-like systems: We shall use \*NIX (Linux, BSD, etc.) for almost all research activities, e.g., calculation, computing, building Proof-of-Concept systems, writing papers, presentation, etc. This is because most of architectural elements in network systems adopt *NIX systems.

  何をするにも\*NIX環境 (Linux, BSD他) を利用します。計算、PoCシステムの構築、論文執筆、プレゼン等々。これは、ネットワークシステムの構成要素はほぼ*NIXシステムを採用しているためです。

- LaTeX: We mostly use LaTeX to write papers and make presentation slides for better visualized mathematical expressions. If possible, we shall make our papers, slides and their sources public.

  論文執筆やプレゼン資料作成には、より「マシ」な数式表現のためにLaTexを使います。可能な場合は、論文・スライド・ソースコードなどを公開していく予定です。

- Modern tools and cloud services: We import approaches in modern 'software development' to our research activities, and use software development tools and cloud services to accelerate our 'online-collaborative' research work.  we shall use Git (and GitHub), AWS/Azure/GCP, VPS (virtual private server), CI (continuous integration services like CircleCI) etc.

  無駄を省くために、モダンなツールやクラウドサービスを利用します。ソフトウェア開発の手法を活用して、(オンラインかつ)共同での論文執筆や研究活動を加速します。例えば、Git (およびGitHub) の積極的な利用や、AWS/Azure/GCP、VPSやCIの利用など。

---

## Contact 連絡先

- E-mail: kurihara (at) ai (dot) u (hyphen) hyogo (dot) ac (dot) jp
- 住所: 650-0047 兵庫県神戸市中央区港島南町7丁目1番28 計算科学センタービル
- Address: Computational Science Center Building, 7-1-28 Minatojima-minamimachi, Chuo-ku, Kobe, Hyogo 650-0047, Japan
