# PHP関数

- `mb_convert_case` : PHP の mbstring 拡張に含まれる関数
  - 「文字列の大文字・小文字変換」を マルチバイト文字（日本語など）対応で行うための関数
  - 通常の strtolower や strtoupper は英字専用ですが、mb_convert_case は UTF-8 などのマルチバイト文字でも安全に使える