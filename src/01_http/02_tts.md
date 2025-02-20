# 音声合成する 

HTTP APIでは、`POST /audio_query` を送り、返された音声クエリを `POST /synthesis` に送ることで音声合成を行います。

```python
~import requests
~
# 話者の一覧を取得する
speakers = requests.get("http://localhost:50021/speakers").json()
print(speakers)
# =>
#  [
#    {
#      "name": "四国めたん",
#      "speaker_uuid": "7ffcb7ce-00ec-4bdc-82cd-45a8889e43ff",
#      "styles": [
#        {"name": "ノーマル", "id": 2},
#        ...
#      ]
#    },
#    ...
#  ]
speaker = speakers[0]["styles"][0]["id"]

# 音声クエリを取得する
audio_query = requests.post(
    "http://localhost:50021/audio_query",
    params={
        "text": "こんにちは",
        "speaker": speaker,
    }
).json()
# =>
#  {
#    "accent_phrases": [
#      {
#        "accent": 5,
#        "moras": [
#          {
#            "consonant": "k",
#            "consonant_length": 0.0733351930975914,
#            "vowel": "o",
#            "vowel_length": 0.12275893241167068,
#            "pitch": 5.746835708618164,
#            "text": "コ"
#          },
#          ...
#        ],
#        "pause_mora": None
#      },
#      ...
#    ],
#    "intonationScale": 1.0,
#    "kana": "コンニチワ'",
#    "outputSamplingRate": 24000,
#    "outputStereo": False,
#    "pitchScale": 0.0,
#    "postPhonemeLength": 0.1,
#    "prePhonemeLength": 0.1,
#    "speedScale": 1.0,
#    "volumeScale": 1.0
#  }

# 音声クエリを合成する
audio = requests.post(
    "http://localhost:50021/synthesis",
    params={
        "speaker": speaker,
    },
    json=audio_query,
).content

# 音声を保存する
with open("audio.wav", "wb") as f:
    f.write(audio)
```
