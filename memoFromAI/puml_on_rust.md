# RustでPlantUMLを利用したい

RustでPlantUMLを利用するには、いくつかの方法があります。主な方法とその手順、使用するクレート、考慮事項をまとめます。

**1. `plantuml` クレートを使う**

これが最も一般的で、使いやすい方法です。`plantuml` クレートはPlantUMLのコマンドラインインターフェースをRustから呼び出すためのクレートです。

* **インストール:**

  ```bash
  cargo add plantuml
  ```

* **使用例:**

  ```rust
  use plantuml::PlantUML;

  fn main() {
      let plantuml = PlantUML::new("
          @startuml
              Alice -> Bob: Hello
              Bob --> Charlie: World
          @enduml
      ");

      plantuml.render("diagram.png").expect("Failed to render diagram");
  }
  ```

  このコードは、PlantUMLの簡単なシーケンス図を生成し、`diagram.png`というファイルに保存します。

* **メリット:**
  * シンプルで使いやすい
  * PlantUMLコマンドラインインターフェースをラップしているため、複雑な設定を気にせずに利用できる
  * 比較的多くの例とドキュメントが提供されている

* **デメリット:**
  * PlantUMLコマンドラインインターフェースに依存するため、PlantUMLのバージョンに依存する可能性がある
  * より高度な設定やカスタマイズが必要な場合は、直接PlantUMLコマンドラインインターフェースを呼び出す方が柔軟性がある

**2. `puml` クレートを使う**

`puml` クレートは、PlantUMLの構文を解析し、画像を生成するためのクレートです。`plantuml` クレートよりも低レベルですが、より柔軟な制御が可能です。

* **インストール:**

  ```bash
  cargo add puml
  ```

* **使用例:**

  ```rust
  use puml::Puml;

  fn main() {
      let puml = Puml::new("
          @startuml
              Alice -> Bob: Hello
              Bob --> Charlie: World
          @enduml
      ");

      puml.render("diagram.png").expect("Failed to render diagram");
  }
  ```

  このコードは、`plantuml` クレートを使った例とほぼ同じです。

* **メリット:**
  * より細かな制御が可能
  * PlantUMLの構文を直接解析するため、PlantUMLのバージョンに依存しにくい

* **デメリット:**
  * `plantuml` クレートよりも複雑
  * PlantUMLの構文に関する知識が必要

**3. PlantUML コマンドラインインターフェースを直接呼び出す**

PlantUMLのコマンドラインインターフェースを、Rustプログラムから直接呼び出す方法です。

* **手順:**
    1. PlantUMLをシステムにインストールする（例: `apt-get install plantuml` や `brew install plantuml`）。
    2. Rustプログラムで、`std::process::Command` を使用してPlantUMLコマンドを実行する。

* **使用例:**

  ```rust
  use std::process::Command;

  fn main() {
      let diagram = "
          @startuml
              Alice -> Bob: Hello
              Bob --> Charlie: World
          @enduml
      ";

      let output = Command::new("plantuml")
          .arg("diagram.txt") // PlantUMLの構文をファイルに保存
          .arg(diagram)
          .output()
          .expect("Failed to execute plantuml");

      if !output.status.success() {
          println!("Error: {:?}", output);
      }
  }
  ```

* **メリット:**
  * PlantUMLのバージョンに依存しない
  * PlantUMLのすべての機能を利用できる

* **デメリット:**
  * PlantUMLのインストールと設定が必要
  * PlantUMLコマンドの引数を正しく設定する必要がある
  * エラー処理が煩雑になる可能性がある

**選ぶべきクレート:**

* **初学者:** `plantuml` クレートが最も簡単で使いやすいでしょう。
* **柔軟性が必要な場合:** `puml` クレートやPlantUMLコマンドラインインターフェースを直接呼び出す方法を検討してください。

**考慮事項:**

* **PlantUMLのバージョン:** `plantuml` クレートと `puml` クレートは、PlantUMLのバージョンに依存する可能性があります。
* **エラー処理:** PlantUMLの構文が間違っている場合など、エラーが発生する可能性があります。適切なエラー処理を実装してください。
* **ファイルパス:** PlantUMLの出力ファイルへのファイルパスを正しく設定してください。
* **依存関係:** 使用するPlantUMLバージョンに応じて、他の依存関係が必要になる場合があります。

**ドキュメント:**

* **plantuml クレート:** [https://crates.io/crates/plantuml](https://crates.io/crates/plantuml)
* **puml クレート:** [https://crates.io/crates/puml](https://crates.io/crates/puml)
* **PlantUML:** [https://plantuml.com/](https://plantuml.com/)

上記の情報をもとに、あなたのニーズに合った方法を選んで、RustでPlantUMLを活用してください。
