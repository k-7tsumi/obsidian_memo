Pythonチュートリアル：https://docs.python.org/ja/3.13/tutorial/
Pythonの用語集：https://docs.python.org/ja/3.13/glossary.html#glossary

## メモ
- インタプリタ言語とは：https://e-words.jp/w/%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%97%E3%83%AA%E3%82%BF%E5%9E%8B%E8%A8%80%E8%AA%9E.html
- これで対話モードになるんだ。ほげ〜
```
$python3.13

Python 3.13.5 (main, Jun 19 2025, 13:53:43) [Clang 14.0.0 (clang-1400.0.29.202)] on darwin

Type "help", "copyright", "credits" or "license" for more information.

**>>>**
```
- [`print()`](https://docs.python.org/ja/3.13/library/functions.html#print "print") 関数を使うと、両端のクォートがなくなり、エスケープされた文字や特殊文字が表示されるため、より読みやすい形で出力できます。

次はここから(連続して並んでいる複数の _文字列リテラル_ (つまり、引用符に囲われた文字列) は、自動的に連結されます。)：https://docs.python.org/ja/3.13/tutorial/introduction.html

- `for` または `while` ループ中の `break` 文は `else` 節と対になる場合があります。`break` を実行せずにループが終了すると、`else` 節が実行されます。
- Pythonのpass文とは：https://note.nkmk.me/python-pass-usage/
- Pythonのスタイルガイド：https://peps.python.org/pep-0008/
- Python の __name__ == '__main__' とは何か知りたい：https://qiita.com/taro-hida/items/c8ca8ba1912243a68369
- 一般的には、モジュールやパッケージから `*` を import するというやり方には賛同できません。
- Python の __init__.py とは何なのか：https://qiita.com/msi/items/d91ea3900373ff8b09d7
	- よく理解できた。
- Python 書式指定一覧：https://qiita.com/puchi2121/items/c96219a731106c44316e
- Pythonのwith文の正体：https://zenn.dev/k41531/articles/9c566a778b79ca
- object.__init__(_self_[, _..._])：https://docs.python.org/ja/3.13/reference/datamodel.html#object.__init__
- ほとんどの Python コードが従っている慣習があります。アンダースコアで始まる名前 (例えば `_spam`) は、 (関数であれメソッドであれデータメンバであれ) 非 public なAPIとして扱います。これらは、予告なく変更されるかもしれない実装の詳細として扱われるべきです。
- `dataclasses`：https://docs.python.org/ja/3.13/library/dataclasses.html#module-dataclasses
- osモジュール：https://docs.python.org/ja/3.13/library/os.html#module-os
- `shutil` --- 高水準のファイル操作：https://docs.python.org/ja/3.13/library/shutil.html#module-shutil
- 現代のPython開発では、仮想環境の使用はほぼ必須とされています。「プロジェクトを始めたらまず仮想環境を作る」が基本的な流れになっています。← そうなんだ。

ひと通りPythonのドキュメントは読んだので、LangChainのAIエージェントを作りつつ、実践でPythonを書いていこう。