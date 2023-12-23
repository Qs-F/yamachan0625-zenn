---
title: 'ドメインモデリング'
---

# ドメインモデリングとは

ドメインモデリングとは、特定の問題領域や業務領域を理解し、それを表現するための構造化された**モデルや図を作成するプロセス**のことを指します。このモデリングの目的は、業務の要件やビジネスルールを明確にし、それをソフトウェアの設計や実装に反映するための準備や基盤の作成をすることです。

ドメインモデリングの過程で欠かせないのが**ドメインエキスパート**の存在です。ドメインエキスパートとは、**特定のドメインや業務領域において深い知識や経験を持つ専門家**のことを指します。 彼らはその領域に関する具体的な業務フローや隠れたビジネスルールに精通しています。彼らの知識を活用することで、実際の業務ニーズに合致した、より正確で効果的なドメインモデルの構築が可能となり、ソフトウェア開発の質や効率も大きく向上するのです。

それでは、ドメインモデリングによって何を決定していくのか、概要を確認しましょう。

## コアドメイン、サブドメインの特定

ひとつはコアドメイン、サブドメインの特定です。これは非常に重要な作業であり、ドメインエキスパートやビジネスステークホルダーと共に十分に議論をする必要があります。

**コアドメイン**とは、各ビジネスが持つ最も重要な価値や競争優位性を生み出す部分を指します。これは、ビジネスの中心的なミッションや戦略に直結しています。

**サブドメイン**とは、ビジネスの主要な業務領域をサポートする部分を指します。これは通常、コアドメインを補完する形で存在します。サブドメインは一般的に、**汎用サブドメイン**と**支援サブドメイン**の 2 種類に分けられます。汎用サブドメインは全体で共通の機能 (認証機能など) を持ち、支援サブドメインはコアドメインをサポートするような機能を持っています。

以下に具体例をあげます。
| ビジネス | コアドメイン | サブドメイン |
|------------------------------|-----------------------------------------------------|------------------------------------------------------------|
| **オンライン書店** | **書籍の販売**<br>これは、オンライン書店の主要な収益源であり、顧客への価値提供の中心となる活動です。 | **在庫管理**<br>書籍の在庫状況を確認し、再注文や供給の最適化を行います。<br>**顧客の購入履歴管理**<br>顧客の購入傾向を理解し、推奨される商品やマーケティング活動を計画します。 |
| **病院の患者管理システム** | **患者の情報管理**<br>患者の健康情報や治療履歴の正確な管理は、病院の診断や治療の品質に直接影響するため、最も重要です。 | **予約・受診のスケジューリング**<br>患者と医師のスケジュールを調整し、効率的な診療の流れを確保します。<br>**診断や治療の記録**<br>患者の治療経過を追跡し、適切な医療サービスを提供します。 |
| **銀行の口座管理システム** | **顧客の口座管理**<br>顧客の資産の安全な管理や取引の履歴の正確な追跡は、銀行の信頼性やサービスの質を保証する中心的な業務です。 | **振込・引き落としの処理**<br>顧客の取引をスムーズに行うためのサービスを提供します。<br>**利息の計算**<br>顧客の預金やローンの利息を適切に計算し、正確な金額を顧客に通知します。 |

## 境界づけられたコンテキストの定義

**境界づけられたコンテキストとは、特定のドメインモデルが適用される明確な境界**を示すものです。通常 1 つのコアドメインもしくはサブドメインに対して 1 つの境界づけられたコンテキストが対応することになります。この境界の中で、一貫した意味を持**つユビキタス言語（共通言語）** が使用され、それに伴うドメインのルールや操作が実装されます。

境界づけられたコンテキストの主な目的は、大規模なシステムや組織内で同じ言葉が異なる意味で使用されることを防ぐことです。これにより、混乱を避けると同時に、モデルの整合性とその適用範囲を明確にすることができます。

例えば、オンライン書店では、書籍の検索、購入、レビューや推薦など多くの異なるプロセスが存在します。それぞれのプロセスでは、「書籍」という言葉が持つ意味や関連情報が異なる場合があります。書籍の購入や検索、閲覧のコンテキストでは、「書籍」はそのジャンルや著者、出版年と関連づけられます。一方で、在庫管理や発注のコンテキストでは、「書籍」は在庫数、再注文の必要性、仕入れ先などと関連づけられるでしょう。

こうした各プロセスを「境界づけられたコンテキスト」として定義することで、それぞれのコンテキスト内で「書籍」という言葉が持つ具体的な意味や関連情報を明確にし、システム全体の混乱を防ぐことができます。そして、各境界づけられたコンテキストは独立したチームによって管理・開発することが容易になります。

## エンティティの特定

**エンティティ**とは、システム内の重要なオブジェクトで、**一意の識別子**によって識別され、変更可能な状態を持つものです。

:::message
ドメイン駆動設計 (DDD) のエンティティとデータベース（DB）のエンティティは混同しがちですが、異なる文脈と目的で用いられるため、その特性や役割に違いがあります。以下、その主な違いを補足します。
| | DDD のエンティティ | DB のエンティティ |
|:----------:|:-------------------:|:------------------:|
| **目的** | ドメインモデルの一部として、ビジネスのロジックやビジネスルールを表現するためのもの。それにより、実際のビジネスの要求や振る舞いを正確にモデル化することを目的とする。 | データの永続的な保存や検索、更新を効率的に行うための構造を定義。データの整合性や効率的なアクセスを目的とする。 |
| **ライフサイクル** | ビジネスの操作やビジネスロジックに従って変更される。 | CRUD（作成、読み取り、更新、削除）の操作によってデータが変更される。 |
| **依存関係** | ドメインのロジックやビジネスのルールに基づいて設計されるため、技術的な詳細や実装から独立。 | 物理的なデータストレージと密接に関連し、特定のデータベース技術やスキーマ設計に依存することが多い。 |

要するに、ドメイン駆動設計のエンティティは**ビジネスの要求やロジックを中心**に考えられるのに対して、`DB` のエンティティは**データの保存や操作を中心**に考えられます。

ドメイン駆動設計のエンティティと `DB` のエンティティの構造が同一になることはよくあります。ですが、それぞれ別の概念であり、依存するべきではないということを理解しておくことが重要です。
:::

### エンティティを特定する際のステップ

1. **ビジネス要件の確認**  
   最初にどのような**ビジネス要件**を満たす必要があるのかを確認します。これによって、システムがどのような情報を保持・操作する必要があるのかが明確になります。

2. **エンティティの識別**  
   保持する必要があるデータや情報の中から、一意の識別子を持つものやライフサイクルを持つものをエンティティとして識別します。例として、オンライン書店の主要なエンティティとしては「**書籍**」が考えられます。

3. **エンティティ間の関連の定義**  
   エンティティ同士の関連性を定義します。例として、オンライン書店では、「**書籍**」エンティティの他に「**著者**」エンティティ、「**出版社**」エンティティなどが考えられます。「一人の著者が複数の書籍を執筆している」というような各エンティティの関連性を見つけます。

4. **属性の識別**  
   各エンティティが持つべき属性や情報を特定します。これはそのエンティティがどのような情報を持つべきか、またどのような操作をサポートすべきかを定義するのに役立ちます。  
   例として、オンライン書店では、「**書籍**」エンティティは**一意**の「**ISBN コード**」を持ち、さまざまな属性 (タイトル、価格、著者など) を保持します。

:::message
エンティティの詳しい説明は`chapter09 エンティティ`で行います。この段階では完全に理解する必要はありません。
:::

## ビジネスルールの把握

ビジネスルールとは、**ビジネスの運営に必要なルールや手続きを表現する明確な規則や条件**のことを指します。これらのルールは、ビジネスの取り決めや規定、戦略に基づいて定義され、システム設計や実装においてそのビジネスの要求を満たすための重要な指針となります。

例として、オンライン書店では、「**一冊の書籍には一つの ISBN コードのみが割り当てられる**」というドメイン特有のルールがあります。このルールをドメインモデリングの段階で発見し、ドメインモデルに反映することで書籍に複数の ISBN コードが割り当てられることを防ぐことができます。

ビジネス環境や市場の変化、組織の戦略の変更などにより、ビジネスルールが変更される可能性は十分に考えられます。そのため、**定期的な見直し**を行い、常に最新の状態を保つことが重要です。

# まとめ

- ドメインモデリングは、業務領域を理解し表現するモデル作成プロセスで、ドメインエキスパートの知識が鍵となる。
- コアドメイン、サブドメイン、境界づけられたコンテキスト、エンティティはドメインモデリングにおいて重要な概念である。

本章では、ドメインモデリングの概要について学びました。
次章では、より実践的なドメインモデリングの手法として「イベントストーミング」を紹介し、このテクニックを利用してドメインを深く理解し、効果的なモデルを構築する方法について探っていきます。