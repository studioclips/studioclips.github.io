# unity-excel-importerの使い方

[unity-excel-importer](https://github.com/mikito/unity-excel-importer)の置き場所

1. エクセルを作成。
1. シート名はエクセルファイル名と違う名前にする（必ず設定）。
1. １行目に半角英数で、スペースを入れないようにして名前を定義（ラベル）。
1. 各定義の下にデータを入れる。
1. エクセルの最初の行に「#」の入っている行はコメントとして処理されない。
1. 保存後、エクセルファイルを Unity 上から選択肢右クリック
1. 「Create」→「ExcelAssetScript」を選択肢管理しているフォルダを選択して保存する（管理ファイル）。
1. ScriptableObject で管理するデータを保存するデータを記入するファイルを用意する（データリストテーブル）。
1. データリストテーブルには通常の型のほか enum も使用できる。
1. 管理ファイルを修正する。<br>
public List<データリストテーブル> エクセルシート名;
1. 一通りファイルを生成したら、エクセルを右クリックして Reimport を選択すると ScriptableObject が生成される。
1. 呼び出しは次の通り。<br>
ScriptableObjectファイル名 変数;<br>
変数.エクセルシート名[選択番号].メンバー

---

## 例）excel ファイル名が sample.xlsx の場合<br>
シート名は sampleEntitis;<br>
データは id(int), name(string)として

```
[SampleEntity.cs（定義ファイル）]

using System;

[Serializable]
public class SampleEntity
{
    public int id;
    public string name;
}
```

```
[sample.cs(生成されるファイル)]

using System.Collections.Generic;
using UnityEngine;

[ExcelAsset]
public class sample : ScriptableObject
{
	//public List<EntityType> Sheet1; // Replace 'EntityType' to an actual type that is serializable.
	public List<SampleEntity> SampleEntities;
}
```

```
[gameControlなど]

public class gameControl : Monobehavior
{
	[SeritalizeField]
	private sample _sampleObj;

	void Start()
	{
		//	id:name のリストをコンソールに表示
		_sampleObj.ForEach(x => Debug.Lod(x.id + ":" + x.name));
	}
}

```