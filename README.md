# wicket-practice
冬休みの課題を取り組む際、今までの知識だけだと限界があるので、今回はそれを補うための勉強会です。

今日の勉強会のルール

- 絶対にコピペはしないこと！！

![kessyoubantyan](https://github.com/Yoshiki-Yamada/wicket-practice/blob/master/image01.jpg)  
https://www.animatetimes.com/news/details.php?id=1532660336



## まず初めに...
まずは、エラーを読めるようになりましょう。

これから、エラーは必ず引き起こすものです。それを読めないと解決はしません。なので、まずはその演習からしたいと思います。

### よくみるエラー集
今日の講習中に出てきたエラーとかまとめてみるか！

|エラー文|内容|
|---|---|
|||


## Apache Wicket
 
 コンポーネント指向の復習
 
### Model
 
### Panel
 panelは、コンポーネント指向の中でも比較的特徴的である。
 
使い方は、`Panel`を`extends`するだけです。

```java
class Lecture extends Panel{
  //Panelの処理をかく
}
```

Panelの処理内容としては、毎回、表示させたいもの、（MenuBar的な）をPanelにするといいです。

 
### TextField
 これは、入力するコンポーネントです。よく使うと思いますので参考に。
 
 書き方は、
 
 ```java
  public class FormPage extends WebPage {
	private static final long serialVersionUID = 1L;

	// name の値を格納するModel
	private IModel<String> nameModel;
	
	/**
	 * コンストラクタ.
	 */
	public FormPage() {
		nameModel = Model.of("");

		Form<Void> form = new Form<Void>("form") ;
		add(form);

		// name を入力する input type="text" 用のコンポーネント
		TextField<String> nameField = new TextField<>("name", nameModel);
		form.add(nameField);
	}
}
 ```
 
 
### Link(復習)

Linkは、クリックしたときの処理を実装するためのコンポーネントです。

書き方は、

```

```


### Form(復習)

#### java
 
 ```java
 public class FormPage extends WebPage {
	private static final long serialVersionUID = 1L;

	// name の値を格納するModel
	private IModel<String> nameModel;
	
	/**
	 * コンストラクタ.
	 */
	public FormPage() {
		nameModel = Model.of("");

		// Formタグ用の Form コンポーネント
		Form<Void> form = new Form<Void>("form") {
			private static final long serialVersionUID = 1L;

			@Override
			protected void onSubmit() {
				// submit ボタンがクリックされた時の処理
				super.onSubmit();
				System.out.println("name : " + nameModel.getObject());
				System.out.println("launch : " + lunchModel.getObject());
			}
		};
		add(form);

		// name を入力する input type="text" 用のコンポーネント
		TextField<String> nameField = new TextField<>("name", nameModel);
		form.add(nameField);
	}
}
 ```
 
#### HTML
 
 ```
 <!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
  <meta charset="UTF-8">
  <title>FormPage</title>
</head>
<body>
<h2>入力フォームを作る</h2>
<form wicket:id="form">
  氏名：<input type="text" wicket:id="name">
  <button type="submit">データ送信</button>
</form>
</body>
</html>
 ```
 
### Form応用
#### java

```
public class FormPage extends WebPage {
	private static final long serialVersionUID = 1L;

	// name の値を格納するModel
	private IModel<String> nameModel;
	
	// lunche の値を格納するModel
	private IModel<String> lunchModel;
	lunchModel = Model.of("");
	
	/**
	 * コンストラクタ.
	 */
	public FormPage() {
		nameModel = Model.of("");

		// Formタグ用の Form コンポーネント
		Form<Void> form = new Form<Void>("form") {
			private static final long serialVersionUID = 1L;

			@Override
			protected void onSubmit() {
				// submit ボタンがクリックされた時の処理
				super.onSubmit();
				System.out.println("name : " + nameModel.getObject());
			}
		};
		add(form);

		// name を入力する input type="text" 用のコンポーネント
		TextField<String> nameField = new TextField<>("name", nameModel);
		form.add(nameField);
		
		// ラジオボタンの選択肢を準備
List<String> lunches = Arrays.asList("鶏唐揚げ定食", "鳥かつ定食", "鳥ガーリック定食");
// 選択肢である lunches を格納するModel. List オブジェクト用には ListModel を使う
IModel<List<String>> lunchesModel = Model.ofList(lunches);

// lunches から一つを選択する radio ボタン用のコンポーネント
RadioChoice<String> radioChoice = new RadioChoice<>("lunch", lunchModel, lunchesModel);
form.add(radioChoice);
	}
}
```

#### html

```
 <!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
  <meta charset="UTF-8">
  <title>FormPage</title>
</head>
<body>
<h2>入力フォームを作る</h2>
<form wicket:id="form">
  氏名：<input type="text" wicket:id="name">
  		<div wicket:id="lunch"></div>
  <button type="submit">データ送信</button>
</form>
</body>
</html>
```

### ListView

#### java

```
package com.example.listView;

import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.list.ListItem;
import org.apache.wicket.markup.html.list.ListView;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;

import java.util.Arrays;
import java.util.List;

/**
 * リストのデータを一覧で表示するページの例.
 */
public class ListViewPage extends WebPage {
	private static final long serialVersionUID = 1L;

	/**
	 * コンストラクタ.
	 */
	public ListViewPage() {
		// 一覧で表示したいデータをListで用意する
		List<String> prefectures = Arrays.asList("北海道", "青森", "岩手", "秋田", "宮城", "福島");

		// リスト用のモデルは、ListModelを使う。
		// ListModelは、Model.ofList(List) や、new ListModel(List) でインスタンス化できる。
		IModel<List<String>> prefecturesModel = Model.ofList(prefectures);

		// Listを一覧で表示する ListView コンポーネントを生成する
		ListView<String> prefecturesView = new ListView<String>("prefectures", prefecturesModel) {
			private static final long serialVersionUID = 1L;

			@Override
			protected void populateItem(ListItem<String> item) {
				// populateItemメソッドには、Listの要素一つ一つに実行する命令を記載する.
				// Listや配列の要素を処理するfor文みたいな役割をしている。

				// ひとつひとつの要素からModelを取り出して、Labelに利用する
				Label prefectureLabel = new Label("prefecture", item.getModel());
				// ListViewの子要素としてコンポーネントを追加するときは、itemを親と考えてaddメソッドを使うことに注意.
				item.add(prefectureLabel);
			}
		};
		add(prefecturesView);

	}

}
```

#### HTML

```

```
 
### Button（復習）



 
### Ajax
 
### FeedBackPanel

#### java

```
package com.example.validation;

import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.RadioChoice;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.model.CompoundPropertyModel;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.util.ListModel;
import org.apache.wicket.validation.validator.StringValidator;
import com.example.beans.UserLunch;

import java.util.Arrays;
import java.util.List;

/**
 * 入力チェックを加えた CPMFormPage.
 */
public class ValidationFormPage extends WebPage {
	private static final long serialVersionUID = 1L;

	/**
	 * コンストラクタ.
	 */
	public ValidationFormPage() {
		IModel<UserLunch> userLunchModel = new CompoundPropertyModel<>(new UserLunch());

		Form<UserLunch> form = new Form<UserLunch>("form", userLunchModel) {
			private static final long serialVersionUID = 1L;

			@Override
			protected void onSubmit() {
				super.onSubmit();
				System.out.println("name : " + getModelObject().getName());
				System.out.println("launch : " + getModelObject().getLunch());
				if (getModelObject().getLunch().equals("鳥ガーリック定食")) {
					error(getModelObject().getName() + "さん、鳥ガーリック定食は売り切れです...");
				} else {
					info(getModelObject().getName() + "さんの注文が完了しました！");
				}
			}
		};
		add(form);

		// エラーメッセージを表示するためのFeedbackPanelを用意
		FeedbackPanel feedbackPanel = new FeedbackPanel("feedback");
		form.add(feedbackPanel);

		TextField<String> nameField = new TextField<String>("name") {
			private static final long serialVersionUID = 1L;

			@Override
			protected void onInitialize() {
				super.onInitialize();
				// テキストフィールドを必須にして、最小3文字最大8文字に設定する。
				setRequired(true);
				add(StringValidator.minimumLength(3));
				add(StringValidator.maximumLength(8));
			}
		};
		form.add(nameField);

		List<String> lunches = Arrays.asList("鶏唐揚げ定食", "鳥かつ定食", "鳥ガーリック定食");
		IModel<List<String>> lunchesModel = new ListModel<>(lunches);

		RadioChoice<String> radioChoice = new RadioChoice<String>("lunch", lunchesModel) {
			private static final long serialVersionUID = 1L;

			@Override
			protected void onInitialize() {
				super.onInitialize();
				// ラジオボタンの選択を必須にする。
				setRequired(true);
			}
		};
		form.add(radioChoice);

	}

}
```

#### HTML

```
<!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
  <meta charset="UTF-8">
  <title>ValidationFormPage</title>
  <style>
    <!--
    .feedbackPanelERROR {
      color: red;
    }

    .feedbackPanelINFO {
      color: green;
    }

    -->
  </style>
</head>
<body>
<h2>入力チェックを使った入力フォームを作る</h2>
<form wicket:id="form">
  <div wicket:id="feedback"></div>
  氏名 : <input type="text" wicket:id="name">
  <div wicket:id="lunch"></div>
  <button type="submit">データ送信</button>
</form>
</body>
</html>
```

#### properties

```
#コンポーネントの名称（ラベル）の設定
form.name=氏名
form.lunch=メニュー
#Validationメッセージの上書き
RangeValidator.minimum='${label}'
```
 
### ModalWindow

#### java

```
```

#### HTML

```
```


