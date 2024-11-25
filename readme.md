
# PyAnnote Speaker Diarization Project

このプロジェクトは、PyAnnoteを使用して話者ダイアリゼーションを実行するための環境と手順を提供します。
1. オフライン事前学習モデルを使用したDiarizationの実行
2. 実行結果描画に確信度を色で表現
3. 既知の話者を辞書登録して推論

---

## 環境構築

1. **Pythonバージョン**:
   - Python 3.11.9 を使用します。
   - 推奨されるツールは VSCode です。

2. **ライブラリ**:
   - 必要なライブラリは `requirements.txt` に記載されています。
   - インストールは以下のコマンドで行います:
     ```bash
     pip install -r requirements.txt
     ```

---

## Diarizationの実行

1. **ノートブック**:
   - `pyannote4SpeakerDiarization.ipynb` を使用します。

2. **ファイル配置**:
   - `inpt` フォルダに以下のファイルを置いてください:
     - 話者ダイアリゼーションを実行する会議動画 (MP4形式など)。
     - リファレンス用のRTTMファイル (評価用)。

3. **実行**:
   - ノートブックを実行すると、話者ダイアリゼーションの処理が行われ、以下の結果が出力されます:
     - **DER評価結果**: 話者識別誤差率 (DER) がコンソールに表示されます。
     - **出力結果**: `otpt` フォルダに以下のファイルが保存されます:
       - 話者ダイアリゼーション結果を可視化した図。
       - ダイアリゼーション結果のRTTMファイル。

---

## 参考情報

### DER (Diarization Error Rate)
DERは、話者ダイアリゼーションの評価指標で、次の3つの要素で構成されます:
- **Missed Speech**: 話者が話しているが検出されなかった部分。
- **False Alarm**: 話者が話していないのに検出された部分。
- **Speaker Confusion**: 話者が誤って識別された部分。

計算式:
```
DER = (Missed Speech + False Alarm + Speaker Confusion) / Total Speech
```

### RTTMファイル
RTTMファイルは、話者ダイアリゼーションの結果を記録するための標準フォーマットです。詳細は以下を参照してください:
- [RTTMファイルについて](#rttmファイルについて)

---

## 参考リンク

- [PyAnnote GitHub リポジトリ](https://github.com/pyannote/pyannote-audio)
- [セグメンテーションモデル (3.0)](https://huggingface.co/pyannote/segmentation)
- [Diarizationモデル (3.1)](https://huggingface.co/pyannote/speaker-diarization)

---

## 出力結果画像の解説

このプロジェクトでは、話者ダイアリゼーションの結果を可視化する図を生成します。以下のコードでプロットが作成されます：

```python
def plot_diarization(diarization, output_image):
    fig, ax = plt.subplots()
    for turn, _, speaker in diarization.itertracks(yield_label=True):
        ax.plot([turn.start, turn.end], [speaker, speaker], lw=6)
    ax.set_xlabel("Time (seconds)")
    ax.set_ylabel("Speaker")
    plt.title("Speaker Diarization")
    plt.savefig(output_image)
```

このプロットの例として、以下の画像が生成されます：

![Speaker Diarization Plot](sample2021short_diarization_plot.png)

### 画像の説明
- 横軸（X軸）は時間（秒）を表します。
- 縦軸（Y軸）は話者を表し、各話者は異なるラベル（例: `EAKER_00`, `EAKER_01`）で示されています。
- 各色の線分は、特定の話者がその時間帯に発話していることを示します。
- この図は、ダイアリゼーション結果を視覚的に確認するためのツールとして役立ちます。
