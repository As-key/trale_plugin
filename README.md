# trale_plugin
This is a project for a import/merge/export plugin and definition files that can be used in TRALE.<br>
It does not include TRALE itself.

このプロジェクトは、TRALE向けのimport/merge/export pluginを扱っています。<br>
TRALE本体のソースコードは含みません。
<br>

作成したpluginファイルは、TRALE本体と同じ場所にあるpluginフォルダに格納してください。TRALE起動時に自動的に定義情報が読み込まれます。</br>

### TRALE Web site :
https://trale.org/
<br><br>


# DLL File

## Plugin interface
Plugin interfaceにそって必要な機能を実装する必要があります。<br>

    namespace TralePluginManager
    {
        public interface InterfaceTralePlugin
        {
            public string GetPluginName();
            public string DefaultName();
            public string ExtentionFilter();
            public void SupportFunction(out bool Import, out bool Merge, out bool Export);
            public bool Import(FileStream ImportFile, out Dictionary<string, List<string>>  OriginalData, out Dictionary<string, List<string>> TranslateData);
            public bool Merge(FileStream MergeFile, ReadOnlyCollection<DataRow> CurrentData, out Dictionary<string, List<string>> TranslateData);
            public bool Export(FileStream ExportFile, ReadOnlyCollection<DataRow> CurrentData, FileStream OriginalFile);
        }
    }
<br>

## public string GetPluginName()
        public string GetPluginName()
        {
            return "plugin for FCPXML";
        }

TRALE上で表示されるPluginの名称を定義します。<br>
他のPluginと競合しないように気を付けてください。<br>
<br>

## public string DefaultName()
        public string DefaultName()
        {
            return "fcpxml_default.fcpxml";
        }

デフォルトファイル名を定義します。<br>
<br>

## public string ExtentionFilter()
        public string ExtentionFilter()
        {
            return "FCPXML files(*.fcpxml)|*.fcpxml|All files(*.*)|*.*";
        }

ファイルダイアログに設定する拡張子情報を定義します。<br>
<br>

## public void SupportFunction(out bool Import, out bool Merge, out bool Export)
        public void SupportFunction(out bool Import, out bool Merge, out bool Export)
        {
            Import = true;
            Merge = false;
            Export = true;
        }

Pluginの機能情報を定義します。
Pluginがサポートする機能にTrueを設定してください。<br>
<br>

## public bool Import(...)

    public bool Import(
        FileStream ImportFile, 
        out Dictionary<string, List<string>> OriginalData, 
        out Dictionary<string, List<string>> TranslateData)

原文Import用のinterfaceです。<br>

    FileStream ImportFile : 
        TRALE上でユーザーが選択したインポートファイルのファイルストリーム

    out Dictionary<string, List<string>> OriginalData :
        原文を格納する為のDictionary型の変数です。

    out Dictionary<string, List<string>> TranslateData :
        訳文を格納する為のDictionary型の変数です。
<br>

## public bool Merge(...)
    public bool Merge(
        FileStream MergeFile,
        ReadOnlyCollection<DataRow> CurrentData,
        out Dictionary<string, List<string>> TranslateData);

訳文Merge用のinterfaceです。<br>

    FileStream MergeFile :
        TRALE上でユーザーが選択したマージ用ファイルのファイルストリーム

    ReadOnlyCollection<DataRow> CurrentData :
        TRALEのメモリ上に展開されている現在のデータです。
        データ構造はTRALEドキュメント（準備中）を参照してください。

    out Dictionary<string, List<string>> TranslateData :
        TRALE側に渡すマージ用の訳文データです。
        データ構造はTRALEドキュメント（準備中）を参照してください。

<br>

## public bool Export(...)
    public bool Export(
        FileStream ExportFile,
        ReadOnlyCollection<DataRow> CurrentData,
        FileStream OriginalFile);

Export（オリジナルデータに訳文をマージして新規ファイル保存）用のinterfaceです。<br>

    FileStream ExportFile :
        エクスポートデータの書き込み先となるファイルストリームです。

    ReadOnlyCollection<DataRow> CurrentData :
        TRALEのメモリ上に展開されている現在のデータです。
        データ構造はTRALEドキュメント（準備中）を参照してください。

    FileStream OriginalFile :
        プロジェクト内に保存されている、オリジナルデータです。
        このデータを元に、エクスポートデータの生成を行ってください。
<br>
