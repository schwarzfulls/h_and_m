# h_and_m

## カラムの意味

### article.csv に関して

型: (105_542, 25)
欠損値: `detail_desc`は一部なし(416/105542=0.3%)

| カラム名                               | 説明                                                                             |
| -------------------------------------- | -------------------------------------------------------------------------------- |
| `article_id(Int64)`                    | 各アイテムに固有のID（10桁）。画像や他のデータと紐づけるための主キー。           |
| `product_code(Int64)`                  | 製品コード。色違いやサイズ違いでも同じ製品には同じコードが付けられることがある。 |
| `prod_name(String)`                    | 製品名（例："Short Dress"）。人間が読む商品名。                                  |
| `product_type_no(Int64)`               | 製品タイプの数値ID。                                                             |
| `product_type_name(String)`            | 製品タイプの名称（例："Dress"、"Trousers" など）。                               |
| `product_group_name(String)`           | 製品をより大きなグループに分類したもの（例："Garment Upper body"）。             |
| `graphical_appearance_no(Int64)`       | 見た目のパターン（デザインやプリント）の数値ID。                                 |
| `graphical_appearance_name(String)`    | 見た目のパターン名（例："Solid", "Striped", "All over pattern"）。               |
| `colour_group_code(Int64)`             | 色グループの数値コード。                                                         |
| `colour_group_name(String)`            | 色グループ名（例："Black", "White", "Beige"）。                                  |
| `perceived_colour_value_id(Int64)`     | 明るさのレベルを示す数値ID。                                                     |
| `perceived_colour_value_name(String)`  | 明るさの名前（例："Light", "Dark", "Medium"）。                                  |
| `perceived_colour_master_id(Int64)`    | 主観的な色グループの数値ID。                                                     |
| `perceived_colour_master_name(String)` | 主観的な色グループ名（例："Red", "Blue", "Beige" など）。                        |
| `department_no(Int64)`                 | デパートメント（部門）の数値ID。                                                 |
| `department_name(String)`              | デパートメントの名称（例："Children Accessories", "Ladies Wear" など）。         |
| `index_code(String)`                   | H&M内部のマーケティング・セグメントコード。                                      |
| `index_name(String)`                   | マーケティング・セグメントの名称（例："Young", "Ladieswear" など）。             |
| `index_group_no(Int64)`                | indexグループの数値コード。                                                      |
| `index_group_name(String)`             | indexグループの名前（例："Kidswear", "Ladieswear", "Menswear"）。                |
| `section_no(Int64)`                    | セクションの数値コード。                                                         |
| `section_name(String)`                 | セクション名（例："Womens Nightwear", "Mens Shirts" など）。                     |
| `garment_group_no(Int64)`              | 衣服グループの数値ID。                                                           |
| `garment_group_name(String)`           | 衣服グループの名称（例："Jersey Basic", "Knitwear" など）。                      |
| `detail_desc(String)`                  | 製品の詳細な説明（自然言語テキスト）。モデルによっては特徴量として利用可能。     |

---

### customers

型: customers shape: (1371980, 7)
欠損値: customer_id, postal_code 以外あり
FN: 895050/1371980 = 65%
Active: 907576/1371980 = 66%
club_member_status: 6062/1371980 = 0.4%
fashion_news_frequency: 16009/1371980 = 1.16%
age: 15861/1371980 = 1.15%

| カラム名                         | 説明                                                                                                                |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `customer_id(String)`            | 各顧客に固有の識別子（匿名化済みの16進文字列）。`transactions.csv` と結びつけて使う。                               |
| `FN(Float64)`                    | 欠損のあるバイナリフラグ（目的不明）。`1` なら何らかの属性あり、`NaN` なら無しと推定される。使わないことも多い。    |
| `Active(Float64)`                | 顧客がアクティブかどうかのフラグ（`1` または `NaN`）。`1` はアクティブな会員とされる。                              |
| `club_member_status(String)`     | 会員ステータス（例：`ACTIVE`, `PRE-CREATE`, `LEFT CLUB` など）。顧客のエンゲージメントレベルを示す。                |
| `fashion_news_frequency(String)` | ファッションニュース（メールなど）をどれくらいの頻度で受け取っているか（例：`NONE`, `REGULARLY`, `MONTHLY` など）。 |
| `age(Int64)`                     | 顧客の年齢。異常値も含まれるため、前処理（分布チェック・クリッピング・標準化など）が必要。                          |
| `postal_code(String)`            | 顧客の居住地域を示す郵便番号（匿名化済み）。地域性に関する分析やクラスタリングの特徴量に使われる。                  |

---

### transactions_train

型: (31788324, 5)
欠損値: なし

| カラム名                  | 説明                                                                                   |
| ------------------------- | -------------------------------------------------------------------------------------- |
| `t_dat(String)`           | 購入日時（`YYYY-MM-DD` 形式の日付）。トランザクションが発生した日付。                  |
| `customer_id(String)`     | 顧客の識別子（匿名化された16進文字列）。`customers.csv` や提出用推薦と紐づける主キー。 |
| `article_id(Int64)`       | 購入された商品のID（10桁）。`articles.csv` に対応し、商品属性と紐づけ可能。            |
| `price(Float64)`          | 購入時の価格（単価、税込、float型）。同じ商品でも購入時期により変動あり。              |
| `sales_channel_id(Int64)` | 購入チャネルの区分：`1` = オンライン、`2` = 実店舗（公式未公開だが、一般的な解釈）。   |

---

### submission.csv

型: shape: (5, 2)

| カラム名              | 説明                                                                      |
| --------------------- | ------------------------------------------------------------------------- |
| `customer_id(String)` | 購入者id                                                                  |
| `prediction(String)`  | MAP12に基づいて12個の商品コード(`product code`)をスペース区切りで出力する |

---

## その他

color = colour: 意味は`色`で同じ(アメリカ英語圏かイギリス英語圏)
