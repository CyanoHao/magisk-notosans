# Noto Sans

Systemlessly apply Noto Sans (for Android N–P).

## Changelog

### 2.000-1

Initial release.

## Credit

[Noto Fonts](https://github.com/googlei18n/noto-fonts) is a font family released by Google. Some metrics (`head.yMin`, `head.yMax`, ) are based on [Roboto](https://github.com/google/roboto)’s metric values, to make Android’s layout engine happy.

## Technical Details


| Metric         | Roboto | Noto Sans (fixed) |
| -------------- | ------ | ----------------- |
| UPM            | 2048   | 1000              |
| `head.yMax`    | 2163   | 1056              |
| `head.yMin`    | -555   | -271              |
| `hhea.ascent`  | 1900   |  928              |
| `hhea.descent` | -500   | -244              |


Here is the script to fix metrics:
```bash
for w in Thin Light Regular Medium Bold Black ThinItalic LightItalic Italic MediumItalic BoldItalic BlackItalic; do
  ttx -t 'head' -t 'hhea' -o $w.ttx NotoSans-$w.ttf
  sed -i 's/yMin value="-[0-9]\+"/yMin value="-271"/; s/yMax value="[0-9]\+"/yMax value="1056"/; s/ascent value="[0-9]\+"/ascent value="928"/; s/descent value="-[0-9]\+"/descent value="-244"/' $w.ttx
  ttx -m NotoSans-$w.ttf -o Roboto-$w.ttf $w.ttx
  rm $w.ttx
done
for w in Light Regular Medium Bold LightItalic Italic MediumItalic BoldItalic; do
  if [[ $w == Regular ]]; then
    notosansfilename=NotoSans-Condensed.ttf
  else
    notosansfilename=NotoSans-Condensed$w.ttf
  fi
  ttx -t 'head' -t 'hhea' -o $w.ttx $notosansfilename
  sed -i 's/yMin value="-[0-9]\+"/yMin value="-271"/; s/yMax value="[0-9]\+"/yMax value="1056"/; s/ascent value="[0-9]\+"/ascent value="928"/; s/descent value="-[0-9]\+"/descent value="-244"/' $w.ttx
  ttx -m $notosansfilename -o RobotoCondensed-$w.ttf $w.ttx
  rm $w.ttx
done
```