# Words App

This folder contains the source code for the Words app codelab.

# Introduction

Words app allows you to select a letter and use Intents to navigate to an Activity that presents a
number of words starting with that letter. Each word can be looked up via a web search.

Words app contains a scrollable list of 26 letters A to Z in a RecyclerView. The orientation of the
RecyclerView can be changed between a vertical list or a grid of items.

The app demonstrates the use of Intents in two ways:

* to navigate inside an app by specifying an explicit destination, and,
* allowing Android to service the Intent using the apps and resources present on the device.

# Pre-requisites

* Experience with Kotlin syntax.
* Able to create an Activity.
* Able to create a RecyclerView and supply it with data.

# Getting Started

1. Install Android Studio, if you don't already have it.
2. Download the sample.
3. Import the sample into Android Studio.
4. Build and run the sample.

# Google公式Kotlinチュートリアル

Intent（明示的・暗黙的）の使い方について。 MainActivityでアルファベットを表示、タップで明示インテントにより詳細画面に移動
詳細画面ではランダムで単語リストが表示。タップして暗黙的インテントによりブラウザ立ち上げ、単語検索 そのほか、メニューバーに用意したボタンで線形レイアウトorグリッドレイアウトを切り替える。

# 明示インテントのメモ

今回の例では、adapter内のonBindViewHolder()にて、クリックリスナーとして画面起動のイベントを登録。

```kotlin
// context取得しておく（遷移先アクティビティ起動に必要のため）
val intent = Intent(context, ExampleActivity::class.java)
intent.putExtras(キー, 遷移先アクティビティに渡す文字列とか)
context.startActivity(intent)
```

キーは、ハードコーディングよりは遷移先Activityにcompanion objectで持たせておくとよい。

# 暗黙的インテントのメモ

`ACTION_VIEW`
以外にも色々と汎用インテントがある([詳細](https://developer.android.com/codelabs/basic-android-kotlin-training-activities-intents?hl=ja&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-kotlin-unit-3-pathway-1%3Fhl%3Dja%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-training-activities-intents#6))。

```kotlin
// context取得しておく（ほかのアプリ起動に必要のため）
val queryUrl: Uri = Uri.parse("${DetailActivity.SEARCH_PREFIX}${item}")
val intent = Intent(Intent.ACTION_VIEW, queryUrl)
context.startActivity(intent)
```

# RecyclerViewのレイアウトマネージャー切り替えメモ

↓こんな感じでRecyclerViewのlayoutManagerプロパティの設定を変えるとレイアウトが変わる。 今回は線形とグリッドレイアウトが切り替わる。

```kotlin
if (isLinearLayoutManager) {
    recyclerView.layoutManager = LinearLayoutManager(this)
} else {
    recyclerView.layoutManager = GridLayoutManager(this, 4)
}
recyclerView.adapter = LetterAdapter()
```

# メニューバーの使い方メモ

- res/menuフォルダ内にメニュー用レイアウトリソース作成
- menuタグ内に、メニューに表示したいアイテムをitemタグで用意
- MainActivityで最低下記のメソッドをオーバーライドする

```kotlin
//メニューレイアウトファイルをインフレート
override fun onCreateOptionsMenu(menu: Menu?): Boolean

// メニューのボタンが選択されたときの処理
override fun onOptionsItemSelected(item: MenuItem): Boolean
```