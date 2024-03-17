# reversWords
Change letter's direction in the words

<?php

class Reverse {
    public string $str1;
    public string $str2;
    public string $str3;
    public string $str4;
    public string $original;
    public array $array;

    public function __construct()
    {
        $this->run();
    }

    public function run()
    {
        $str1 = 'Cat суПер пупЕр Мышь houSe домИК elEpHant ';
        $str2 = 'cat, Зима: is ';
        $str3 = "'cold' ";
        $str4 = 'now это «Так» "просто" third-part can`t';
        $original = $str1 . $str2 . $str3 . $str4;
//        $original = 'Привет, мир! é ٢٦ ﬧ ¼ ¾ Ёёж 13212 ไ  ﬧ _ Hello, world!';
//        $original = 'Д`обро1 пожаловать, в "супер пупер:" «Википедию»! W`elcome2 to, "super  duper:" «Wikipedia»! ¡`Bienvenido3, a la "súper   tonta:" «Wikipedia»!';

        echo $original; echo "<br/>";

        $array = preg_split('# #', $original);

        $reversed = $this->string_reverse($array);

        $changed = $this->change_registr($original, $reversed);
        echo $changed;
    }

    public function change_registr($str1, $str2) : string
    {
        $changed = '';
        $l = mb_strlen($str1, "UTF-8");
        for ($i = 0; $i < $l; $i++)
        {
            $char1 = mb_substr ($str1, $i, 1, "UTF-8");
            $char2 = mb_substr ($str2, $i, 1, "UTF-8");
            if (mb_strtolower($char1, "UTF-8") != $char1) {
                $char2 = mb_strtoupper($char2, 'UTF-8');
            }
            $changed .= $char2;
        }
        return $changed;
    }

    public function string_reverse($array): string
    {
        foreach ($array as $k => $v) {
            /* Слова в двойных горизонтальных кавычках «...» */
            if (preg_match('#«(\S+)»#', $v)) {
                $arr[] = $v;
            } else {
                /* Слова в двойных вертикальных кавычках '' "" */
                if (preg_match('#\'(\S+)\'|\"(\S+)\"#', $v)) {
                    $arr[] = mb_strtolower($this->mb_strrev($v));
                } else {
                    /* Слова с пунктуацией , : . */
                    if (preg_match('#(\S+)([,:\.])$#', $v, $matches)) {
                        $arr[] = mb_strtolower($this->mb_strrev($matches[1])) . $matches[2];
                    } else {
                        /* Составные слова с разделителями - ` */
                        if (preg_match('#(\S+)([-`])(\S+)#', $v, $matches)) {
                            $arr[] = mb_strtolower($this->mb_strrev($matches[1]) . $matches[2] . $this->mb_strrev($matches[3]));
                        } else {
                            /* Составные слова с разделителем   */
                            if (preg_match('#(\S+)([ ])(\S+)#', $v, $matches)) {
                                $l = mb_strlen($matches[1], "UTF-8");
                                $str1 = mb_substr($matches[1], 0, $l - 1, "UTF-8");
                                $str2 = " ";
                                $arr[] = mb_strtolower($this->mb_strrev($str1) . $str2 . $this->mb_strrev($matches[3]));
                            } else {
                                /* Одиночные слова */
                                $arr[] = mb_strtolower($this->mb_strrev($v));
                            }
                        }
                    }
                }
            }
        }
        return implode(' ', $arr);
    }

    public function mb_strrev($string, $encoding = null) : string
    /* https://kvz.io/blog/reverse-a-multibyte-string-in-php.html */
    {
        if ($encoding === null) {
            $encoding = mb_detect_encoding($string);
        }

        $length = mb_strlen($string, $encoding);
        $reversed = '';
        while ($length-- > 0) {
            $reversed .= mb_substr($string, $length, 1, $encoding);
        }

        return $reversed;
    }
}

new Reverse();
