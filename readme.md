# Modular Avatarをつかって着せ替えしたVRChatアバターをGLB化してResoniteに持ち込む試み
 - この文書は Unity 2022.3.6.f1 / VRCSDK 3.5.2 / UniGLTF-v0.121.0 を対象としている
 - Unity 2019.4.31f1 / VRCSDK 3.4.2 / UniVRM-0.99 を対象とした文書は[こちら](https://github.com/konto250/VRMimport/tree/for2019)
 - Unity 2022.3.6.f1 / VRCSDK 3.5.0 / UniVRM-v0.118.0 を対象とした文書は[こちら](https://github.com/konto250/VRMimport/tree/for2022)

## 手順の概略
- [Modular Avatar](https://modular-avatar.nadena.dev/ja/docs/intro)を使いアバターの着せ替えをしてVRChatにアップロード
- ブレンドシェイプの設定値をメモ
- UniGLTF 最新版を使いGLB形式でエクスポートする
- Resoniteにインポート
- メモしたブレンドシェイプの値を設定（GLBにしてResoniteにインポートするどこかの時点で、設定値がすべてゼロになってしまうため）

## これを書いたときの環境など
- 使用アバター  [ルーシュカ](https://booth.pm/ja/items/4296675)（[とるた めるか～と -Torta Mercato-](https://tortamercato.booth.pm/)）
- 使用衣装  [EXTENSION CLOTHING『LITTLE MONSTER』](https://extension.booth.pm/items/4604087)（[EXTENSION CLOTHING](https://extension.booth.pm/)）
- Win11 Pro 23H2 build 22631.3447 / Unity 2022.3.6.f1 / VRCSDK **3.5.2** / UniGLTF-v0.121.0
## 実際の手順

### ➀ [VCC](https://vrchat.com/home/download) でアバターのプロジェクト作成
- Modular Avatar,liltoon(アバター・衣装が他のシェーダーを指定している場合はそれに従う)を追加して作成
- UniGLTF, VRMShaders 最新版のパッケージを追加
  - githubのvrm-c/UniVRMを参照(https://github.com/vrm-c/UniVRM/releases)

![alt_text](images/image0.png "image_tooltip")

### ② アバター、衣装のunitypackageインポート
![alt_text](images/image1.png "image_tooltip")

### ③アバター、衣装のprefabをhierarchyに入れる
![alt_text](images/image2.png "image_tooltip")

### ④ hierarchyの衣装のルートを選択して右クリックして、Modular Avatar > Setup Outfitを選択
![alt_text](images/image3.png "image_tooltip")

### ⑤ メニューバーのVRChat SDK > Show Control Panelを選択して、Build&Publish する(VRChatにアップロードしない人はこの手順は省略してよい)

### ⑥ hierarchyのアバタールートを選択して、メニューバーのTools > Modular Avatar > Manual bake avatar を選択
- アバター名(clone)がhierarchyに追加される
- アバターの（必要なら衣装のも）、ブレンドシェイプの設定値をメモ
- アバターのアーマチュアを展開してリグとして認識させないボーンの名前の先頭に&lt;NOIK&gt;と追記する。ルーシュカの場合はUpperChest。
- 衣装にも&lt;NOIK&gt;の追記が必要となる場合もある

![alt_text](images/image4.png "image_tooltip")

### ⑦ hierarchyのアバター名(clone)を選択して、メニューバーの UniGLTF > Export gLTF を選択
- ExportSettingタブでDivide Vertex Bufferにチェックを入れる
- ExportをクリックしてExport実行（今回は VRMTEST.GLB というファイル名でエクスポート）

### ⑧ GLBファイルが出力される。

### ⑨ VRMTEST.GLB というファイルが出力されているので、これをResoniteにインポート（Resoniteのウィンドウにドラッグ＆ドロップ）する。
モデルインポーターのダイアログが表示されるので、3Dモデル → 一般的なほとんどのモデル → ヒューマノイドの身長に自動設定 → 高度な設定
- マテリアル　XiexeToon
- 「アセットをオブジェクト内に入れる」を☑
- インポート実行を押す

### ⑩ モデルが表示されれば完了

少し待てばテクスチャーも反映されるはずだが数分待っても反映しない場合は、UnityのProjectのAssetフォルダから探しResoniteにインポートしてモデルのXiexetoonマテリアルに設定する（設定方法は省略）

![alt_text](images/image8.jpg "image_tooltip")

### ⑪ アバタークリエイターでアバター化する（省略）
 - Assetスロットの整理（AssetOrganizer）
 - 揺れ物の設定
 - Shadowrampの調整
 - Eye ManagerのMaxSwingの調整
 - アバター保護の設定（SimpleAvva）
 - GLBエクスポートする前にメモしたブレンドシェイプの設定値を、アバターに（必要なら衣装にも）、設定する
 - etc, etc...
#### 参考
- [アバターセットアップマニュアル by lill](https://lillill.notion.site/by-lill-21ba5946cdd841339366b93913a52407)

### ⑫ アバター作者・衣装作者が凄くがんばったマテリアルはResoniteに反映していないので、自分でなんとかするか有識者に聞く
liltoonの色調補正の焼き込み、アルファの焼き込みだけでなんとかなることもある
#### 参考
- [XiexeToonマテリアル完全攻略!](https://note.com/akiram_vr/n/n5e55290e2cee)

## 参考にした情報源
- [VRMアバターについて](https://sharedx.notion.site/VRM-d93c6e3ae2f647e0956054efff1d20b9)
- [Avatar_Import#ボーン、ブレンドシェイプ名の注意点など](https://neosvrjp.memo.wiki/d/Avatar_Import#content_1)
- [VRChat向けfbxモデル「薄荷」をResoniteに導入する方法(glb変換) ](https://note.com/ckkcobalt/n/n9db7c3e8a1f5)
