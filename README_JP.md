# DeprecatedReminder

![Plugin](https://user-images.githubusercontent.com/51815450/188594589-cce05730-fd2c-47b1-afe6-0e521d8d72fc.png)

<!--ts-->
* [概要](#概要)
* [動作環境](#動作環境)
* [インストール](#インストール)
* [機能と使い方](#機能と使い方)
  * [コード](#コード)
  * [コード以外](#コード以外)
    * [アセット](#アセット)
    * [アクター](#アクター)
    * [ノード](#ノード)
    * [コンテキストメニューの使い方](#コンテキストメニューの使い方)
* [コンテンツブラウザフィルタ](#コンテンツブラウザフィルタ)
* [Deprecated Reminder Manager](#Deprecated-Reminder-Manager)
* [メタ指定子](#メタ指定子)
  * [Worker Name Meta Specifier](#Worker-Name-Meta-Specifier)
  * [Description Meta Specifier](#Description-Meta-Specifier)
  * [独自のメタ指定子](#独自のメタ指定子)
* [オプション](#オプション)
* [作者](#作者)
* [履歴](#履歴)
<!--te-->

## 概要

このプラグインはコードやアクターなどに期日を設定し、期日を過ぎたものを管理できるプラグインです。  
期日が過ぎたものをコンパイルやクックの時に警告やエラーとして知らせる機能を追加します。  

## 動作環境

対象バージョン : UE4.26 ～ 5.3    
対象プラットフォーム : Windows

## インストール

[マーケットプレイス](https://www.unrealengine.com/marketplace/ja/product/deprecated-reminder)からインストールしてください。  
プラグインのインストール後に機能が使用できない場合は、プラグインが有効化されていない可能性がありますので、編集 > プラグイン からプラグインの有効のチェックが入っているかをご確認ください。

## 機能と使い方

### コード

このプラグインによって提供される以下のマクロを使用してコードに期日を設定できます。  

```Macro.h
DEPRECATED_REMINDER(Year, Month, Day, Hour, Minute, ...)
```

引数には順に、年、月、日、時、分を設定します。  
さらに、後の項目で説明するメタ指定子を必要に応じて付加することができます。
このマクロはグローバル空間や特定の名前空間、関数やクラスの内部などに配置することができます。  
リテラル内やコメントアウトされたものは無効化されます。

```Exsample.h
DEPRECATED_REMINDER(2022, 10, 20, 19, 00, Description = Macros that are placed in global space and have no workers set);

void Test()
{
	DEPRECATED_REMINDER(2022, 07, 28, 00, 35, WorkerName = "Unreal Engine", Description = "A macro placed inside a function and set to a worker.");
}

#define LITERAL_TEST "DEPRECATED_REMINDER(2022, 04, 30, 15, 58)"

// DEPRECATED_REMINDER(2022, 04, 30, 15, 58);

/*
 * DEPRECATED_REMINDER(2022, 07, 16, 00, 35);
 */

class FTest
{
	DEPRECATED_REMINDER(2022, 07, 16, 00, 35);
};
```

期限切れのコードがあった場合以下のようにコンパイル時に警告やエラーが発生します。

![CodeWarning](https://user-images.githubusercontent.com/51815450/188559977-8686d109-af6a-45db-ad4e-f5beba79f445.png)
![CodeError](https://user-images.githubusercontent.com/51815450/188559993-9e4480c8-b250-4669-abc0-91012aa8cdbd.png)

後に説明するエディタ環境設定から動作を変更することができます。

### コード以外

コード以外のものに関してはエディタ上のコンテキストメニューから期日を設定します。  
期日を設定できるものとコンテキストメニューは以下の通りです。

#### アセット  
![AssetContextMenu](https://user-images.githubusercontent.com/51815450/189822386-03b1efe1-f4e7-4ae4-b624-d68b834514b0.png)

#### アクター  
![ActorContextMenu](https://user-images.githubusercontent.com/51815450/189822377-560a6ff4-9f16-457e-86b9-2f0bbf8c6d75.png)

#### ノード  
![NodeContextMenu](https://user-images.githubusercontent.com/51815450/189822395-b0021595-3781-42ae-b6b0-eb31aab6497c.png)

#### コンテキストメニューの使い方  
![ContextMenu](https://user-images.githubusercontent.com/51815450/189823499-5fd9ac48-3dbd-4e45-a897-d95927d68954.png)

| **番号** | **説明**                                    |
|--------|-------------------------------------------|
| 1      | 期日を指定します。                                 |
| 2      | メタ指定子のデータを入力します。                          |
| 3      | 対象に1で設定した期日でリマインダを設定します。                  |
| 4      | 既に設定されているリマインダの情報を現在1と2に設定されているデータで更新します。 |
| 5      | 設定されているリマインダを削除します。                       |

リマインダは対象物が含まれるパッケージに埋め込まれるため、有効なパッケージがない場合は機能が利用できません。  
例えば、未保存のレベルに配置されたアクターなどです。    
以下のような警告が表示された場合は対象のアセットを保存してから再度コンテキストメニューを開いてください。  

![ContextMenuWarning](https://user-images.githubusercontent.com/51815450/189822415-69d24096-2350-4958-9867-41b1373aec73.png)

期限切れのものがあった場合以下のようにクック時に警告やエラーが発生します。

![AssetWarning](https://user-images.githubusercontent.com/51815450/188565127-d09d1556-8e6a-4a03-a124-601a2cbb4d6b.png)
![AssetError](https://user-images.githubusercontent.com/51815450/188565134-6c0797ac-3cb5-4180-8d20-5dce0744ec6f.png)

後に説明するエディタ環境設定から動作を変更することができます。

## コンテンツブラウザフィルタ  

![ContentBrowserFilter](https://github.com/Naotsun19B/DeprecatedReminder-Document/assets/51815450/5c5acd6c-7902-452e-be4f-c54f87d7a5e3)

リマインダを含むアセットのみを表示するコンテンツブラウザのフィルタが利用できます。  

## Deprecated Reminder Manager

エディタ上でリマインダの一覧を表示し、削除や対象物を開いたりできるマネージャーを利用できます。  
Tools(UE4ではWindow)メニューからマネージャーを起動することができます。  

![OpenDeprecatedReminderManager](https://user-images.githubusercontent.com/51815450/188566087-b9a31207-cb08-4fa9-80af-c194e52a8c87.png)

![DeprecatedReminderManager](https://user-images.githubusercontent.com/51815450/188567751-71cf8af0-ae07-4f8d-82b9-65ef4b3a23ab.png)

| **番号** | **説明**                    |
|--------|---------------------------|
| 1      | 種類や場所、特定のメタ指定子などで検索をかけます。 |
| 2      | フィルターやソートなどの状態をリセットします。   |
| 3      | リマインダが設定されている対象の種類。       |
| 4      | リマインダが設定されている対象のある場所。     |
| 5      | リマインダに設定されている期日。          |
| 6      | リマインダに設定されている期日までの残り時間。   |
| 7      | メタ指定子で設定したデータ。            |

一部の項目はフィルターをかけることができます。

![FilterWithType](https://user-images.githubusercontent.com/51815450/188568906-1a00a0a9-9997-4a6e-9d40-c6f023bc9fa8.png)
![FilterWithDifferenceFromDueDate](https://user-images.githubusercontent.com/51815450/188568915-a56eafcb-e265-40ca-867b-b40b68e5091f.png)

## メタ指定子

デフォルトでいくつかのメタ指定子が組み込まれています。    
また、C++を用いて独自のメタ指定子を追加することもできます。    

### Worker Name Meta Specifier
作業者の名前を設定するメタ指定子です。  
コンテキストメニュー上の入力欄が空の場合はPCのユーザー名を使用します。  

| **クラス名**                                    | **ソースファイルパス**                                                                                                                                 |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderWorkerNameMetaSpecifier  | DeprecatedReminder\Source\DeprecatedReminderEditor\Private\DeprecatedReminderEditor\MetaSpecifiers\Implementations\WorkerNameMetaSpecifier.h  |

### Description Meta Specifier
リマインダを設定した項目についての説明文を設定するメタ指定子です。

| **クラス名**                                    | **ソースファイルパス**                                                                                                                                  |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderDescriptionMetaSpecifier | DeprecatedReminder\Source\DeprecatedReminderEditor\Private\DeprecatedReminderEditor\MetaSpecifiers\Implementations\DescriptionMetaSpecifier.h  |

### 独自のメタ指定子
以下のメタ指定子のベースクラスを継承したクラスを定義します。(登録は自動で行われます)

| **クラス名**                          | **ソースファイルパス**                                                                                                                        |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderMetaSpecifier  | DeprecatedReminder/Source/DeprecatedReminderEditor/Public/DeprecatedReminderEditor/MetaSpecifiers/DeprecatedReminderMetaSpecifier.h  |

継承したクラスで以下の関数を必要に応じてオーバーライドして実装をしてください。

| **関数名**                         | **説明**                                                         |
|---------------------------------|----------------------------------------------------------------|
| GetMetaSpecifierName            | このクラスで定義されたメタ指定子の名前を返します。                                      |
| GetPriority                     | このメタ指定子の優先度を返します。この値の大きい順に拡張処理を行い、優先度の値が等しい場合は名前を比較して順位を決定します。 |
| ExtendDeprecatedReminderManager | Deprecated Reminder Managerで表示されるデータを拡張するかどうかを返します。            |
| GetColumnName                   | Deprecated Reminder Managerで表示される列の名前を返します。                    |
| Compare                         | Deprecated Reminder Managerで並べ替えるときに使用される比較結果を返します。            |
| Contains                        | Deprecated Reminder Managerの検索欄に入力された値にマッチするかどうかを返します。         |
| UseCheckListInHeaderRow         | Deprecated Reminder Managerのヘッダー行にチェックリストを表示するかどうか。            |
| GetCheckListDisplayString       | Deprecated Reminder Managerのヘッダー行のチェックリストに表示するデータを返します。        |
| ExtendRowContextMenu            | Deprecated Reminder Managerのコンテキストメニューを拡張するかどうか。               |
| BuildRowContextMenu             | Deprecated Reminder Managerのコンテキストメニューを拡張します。                  |
| GetRowWidget                    | Deprecated Reminder Managerの行に表示するウィジェットを返します。                 |
| ExtendItemContextMenu           | リマインダーを設定するアイテムのコンテキストメニューを拡張するかどうかを返します。                      |
| GetItemContextMenuWidget        | リマインダーを設定するアイテムのコンテキストメニューに追加するウィジェットとラベルを返します。                |
| ModifyMetaSpecifier             | リマインダーをメタ指定子として設定する前に、ウィジェットで選択された値を書き込みます。                    |

## オプション

![Settings](https://user-images.githubusercontent.com/51815450/188559913-7d9f9c5c-7e21-4686-addb-6dba1197fb9d.png)

| **カテゴリ**     | **項目**                                      | **説明**                                                                                      |
|--------------|---------------------------------------------|---------------------------------------------------------------------------------------------|
| Verbosity    | Deprecated Remind Verbosity At Cook Time    | クック時に期限切れのものがあった際の動作を指定します。                                                                 |
|              | Deprecated Remind Verbosity At Compile Time | コンパイル時に期限切れのコードがあった際の動作を指定します。 (C++プロジェクトでのみ利用可能)                                           |
| Search Scope | Search Into Engine Contents                 | エンジンコンテンツ内のアセットにもリマインダが設定されているかチェックするかどうかを指定します。検索時間が大幅に増加する可能性があるため必要な場合のみご利用ください。         |
|              | Search Into Engine Codes                    | エンジンコードにもリマインダマクロがないかチェックするかどうかを指定します。検索時間が大幅に増加する可能性があるため必要な場合のみご利用ください。(C++プロジェクトでのみ利用可能) |
| Misc         | Warn When Due Date Is Approaching           | 期日が近づいたときに警告するかどうかを指定します。                                                                   |
|              | Remaining Time To Warn                      | 警告が必要な期日までの残り時間を指定します。                                                                      |

## 作者

[Naotsun](https://twitter.com/Naotsun_UE)

## 履歴

- (2023/09/09) v1.4  
  UE5.3に対応しました  

- (2023/05/12) v1.3   
  UE5.2に対応しました  
  リマインダを含むアセットのみを表示するコンテンツブラウザフィルタを追加しました  

- (2022/11/08) v1.2   
  UE5.1に対応しました  

- (2022/10/23) v1.1   
  不要なリビルドが発生しないように修正  
  エディタ上のリマインダーの作成や削除などの操作をUndo/Redoできるように改善  

- (2022/09/17) v1.0   
  プラグインを公開  
