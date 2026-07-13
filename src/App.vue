<script setup>
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'

const kanaCards = [
  { kana: 'あ', readingKo: '아', romaji: 'a' },
  { kana: 'い', readingKo: '이', romaji: 'i' },
  { kana: 'う', readingKo: '우', romaji: 'u' },
  { kana: 'え', readingKo: '에', romaji: 'e' },
  { kana: 'お', readingKo: '오', romaji: 'o' },
  { kana: 'か', readingKo: '카', romaji: 'ka' },
  { kana: 'き', readingKo: '키', romaji: 'ki' },
  { kana: 'く', readingKo: '쿠', romaji: 'ku' },
  { kana: 'け', readingKo: '케', romaji: 'ke' },
  { kana: 'こ', readingKo: '코', romaji: 'ko' },
  { kana: 'さ', readingKo: '사', romaji: 'sa' },
  { kana: 'し', readingKo: '시', romaji: 'shi' },
  { kana: 'す', readingKo: '스', romaji: 'su' },
  { kana: 'せ', readingKo: '세', romaji: 'se' },
  { kana: 'そ', readingKo: '소', romaji: 'so' },
  { kana: 'た', readingKo: '타', romaji: 'ta' },
  { kana: 'ち', readingKo: '치', romaji: 'chi' },
  { kana: 'つ', readingKo: '츠', romaji: 'tsu' },
  { kana: 'て', readingKo: '테', romaji: 'te' },
  { kana: 'と', readingKo: '토', romaji: 'to' },
  { kana: 'な', readingKo: '나', romaji: 'na' },
  { kana: 'に', readingKo: '니', romaji: 'ni' },
  { kana: 'ぬ', readingKo: '누', romaji: 'nu' },
  { kana: 'ね', readingKo: '네', romaji: 'ne' },
  { kana: 'の', readingKo: '노', romaji: 'no' },
  { kana: 'は', readingKo: '하', romaji: 'ha' },
  { kana: 'ひ', readingKo: '히', romaji: 'hi' },
  { kana: 'ふ', readingKo: '후', romaji: 'fu' },
  { kana: 'へ', readingKo: '헤', romaji: 'he' },
  { kana: 'ほ', readingKo: '호', romaji: 'ho' },
  { kana: 'ま', readingKo: '마', romaji: 'ma' },
  { kana: 'み', readingKo: '미', romaji: 'mi' },
  { kana: 'む', readingKo: '무', romaji: 'mu' },
  { kana: 'め', readingKo: '메', romaji: 'me' },
  { kana: 'も', readingKo: '모', romaji: 'mo' },
  { kana: 'や', readingKo: '야', romaji: 'ya' },
  { kana: 'ゆ', readingKo: '유', romaji: 'yu' },
  { kana: 'よ', readingKo: '요', romaji: 'yo' },
  { kana: 'ら', readingKo: '라', romaji: 'ra' },
  { kana: 'り', readingKo: '리', romaji: 'ri' },
  { kana: 'る', readingKo: '루', romaji: 'ru' },
  { kana: 'れ', readingKo: '레', romaji: 're' },
  { kana: 'ろ', readingKo: '로', romaji: 'ro' },
  { kana: 'わ', readingKo: '와', romaji: 'wa' },
  { kana: 'を', readingKo: '오', romaji: 'wo' },
  { kana: 'ん', readingKo: '응', romaji: 'n' },
  { kana: 'が', readingKo: '가', romaji: 'ga' },
  { kana: 'ぎ', readingKo: '기', romaji: 'gi' },
  { kana: 'ぐ', readingKo: '구', romaji: 'gu' },
  { kana: 'げ', readingKo: '게', romaji: 'ge' },
  { kana: 'ご', readingKo: '고', romaji: 'go' },
  { kana: 'ざ', readingKo: '자', romaji: 'za' },
  { kana: 'じ', readingKo: '지', romaji: 'ji' },
  { kana: 'ず', readingKo: '즈', romaji: 'zu' },
  { kana: 'ぜ', readingKo: '제', romaji: 'ze' },
  { kana: 'ぞ', readingKo: '조', romaji: 'zo' },
  { kana: 'だ', readingKo: '다', romaji: 'da' },
  { kana: 'ぢ', readingKo: '지', romaji: 'ji' },
  { kana: 'づ', readingKo: '즈', romaji: 'zu' },
  { kana: 'で', readingKo: '데', romaji: 'de' },
  { kana: 'ど', readingKo: '도', romaji: 'do' },
  { kana: 'ば', readingKo: '바', romaji: 'ba' },
  { kana: 'び', readingKo: '비', romaji: 'bi' },
  { kana: 'ぶ', readingKo: '부', romaji: 'bu' },
  { kana: 'べ', readingKo: '베', romaji: 'be' },
  { kana: 'ぼ', readingKo: '보', romaji: 'bo' },
  { kana: 'ぱ', readingKo: '파', romaji: 'pa' },
  { kana: 'ぴ', readingKo: '피', romaji: 'pi' },
  { kana: 'ぷ', readingKo: '푸', romaji: 'pu' },
  { kana: 'ぺ', readingKo: '페', romaji: 'pe' },
  { kana: 'ぽ', readingKo: '포', romaji: 'po' },
  { kana: 'きゃ', readingKo: '캬', romaji: 'kya' },
  { kana: 'きゅ', readingKo: '큐', romaji: 'kyu' },
  { kana: 'きょ', readingKo: '쿄', romaji: 'kyo' },
  { kana: 'しゃ', readingKo: '샤', romaji: 'sha' },
  { kana: 'しゅ', readingKo: '슈', romaji: 'shu' },
  { kana: 'しょ', readingKo: '쇼', romaji: 'sho' },
  { kana: 'ちゃ', readingKo: '챠', romaji: 'cha' },
  { kana: 'ちゅ', readingKo: '츄', romaji: 'chu' },
  { kana: 'ちょ', readingKo: '쵸', romaji: 'cho' },
  { kana: 'にゃ', readingKo: '냐', romaji: 'nya' },
  { kana: 'にゅ', readingKo: '뉴', romaji: 'nyu' },
  { kana: 'にょ', readingKo: '뇨', romaji: 'nyo' },
  { kana: 'ひゃ', readingKo: '햐', romaji: 'hya' },
  { kana: 'ひゅ', readingKo: '휴', romaji: 'hyu' },
  { kana: 'ひょ', readingKo: '효', romaji: 'hyo' },
  { kana: 'みゃ', readingKo: '먀', romaji: 'mya' },
  { kana: 'みゅ', readingKo: '뮤', romaji: 'myu' },
  { kana: 'みょ', readingKo: '묘', romaji: 'myo' },
  { kana: 'りゃ', readingKo: '랴', romaji: 'rya' },
  { kana: 'りゅ', readingKo: '류', romaji: 'ryu' },
  { kana: 'りょ', readingKo: '료', romaji: 'ryo' },
  { kana: 'ぎゃ', readingKo: '갸', romaji: 'gya' },
  { kana: 'ぎゅ', readingKo: '규', romaji: 'gyu' },
  { kana: 'ぎょ', readingKo: '교', romaji: 'gyo' },
  { kana: 'じゃ', readingKo: '자', romaji: 'ja' },
  { kana: 'じゅ', readingKo: '주', romaji: 'ju' },
  { kana: 'じょ', readingKo: '조', romaji: 'jo' },
  { kana: 'びゃ', readingKo: '뱌', romaji: 'bya' },
  { kana: 'びゅ', readingKo: '뷰', romaji: 'byu' },
  { kana: 'びょ', readingKo: '뵤', romaji: 'byo' },
  { kana: 'ぴゃ', readingKo: '퍄', romaji: 'pya' },
  { kana: 'ぴゅ', readingKo: '퓨', romaji: 'pyu' },
  { kana: 'ぴょ', readingKo: '표', romaji: 'pyo' },
]

const hiraganaChartSections = [
  {
    title: '기본 오십음',
    rows: [
      [
        { kana: 'あ', romaji: 'a', readingKo: '아' },
        { kana: 'い', romaji: 'i', readingKo: '이' },
        { kana: 'う', romaji: 'u', readingKo: '우' },
        { kana: 'え', romaji: 'e', readingKo: '에' },
        { kana: 'お', romaji: 'o', readingKo: '오' },
      ],
      [
        { kana: 'か', romaji: 'ka', readingKo: '카' },
        { kana: 'き', romaji: 'ki', readingKo: '키' },
        { kana: 'く', romaji: 'ku', readingKo: '쿠' },
        { kana: 'け', romaji: 'ke', readingKo: '케' },
        { kana: 'こ', romaji: 'ko', readingKo: '코' },
      ],
      [
        { kana: 'さ', romaji: 'sa', readingKo: '사' },
        { kana: 'し', romaji: 'shi', readingKo: '시' },
        { kana: 'す', romaji: 'su', readingKo: '스' },
        { kana: 'せ', romaji: 'se', readingKo: '세' },
        { kana: 'そ', romaji: 'so', readingKo: '소' },
      ],
      [
        { kana: 'た', romaji: 'ta', readingKo: '타' },
        { kana: 'ち', romaji: 'chi', readingKo: '치' },
        { kana: 'つ', romaji: 'tsu', readingKo: '츠' },
        { kana: 'て', romaji: 'te', readingKo: '테' },
        { kana: 'と', romaji: 'to', readingKo: '토' },
      ],
      [
        { kana: 'な', romaji: 'na', readingKo: '나' },
        { kana: 'に', romaji: 'ni', readingKo: '니' },
        { kana: 'ぬ', romaji: 'nu', readingKo: '누' },
        { kana: 'ね', romaji: 'ne', readingKo: '네' },
        { kana: 'の', romaji: 'no', readingKo: '노' },
      ],
      [
        { kana: 'は', romaji: 'ha', readingKo: '하' },
        { kana: 'ひ', romaji: 'hi', readingKo: '히' },
        { kana: 'ふ', romaji: 'fu', readingKo: '후' },
        { kana: 'へ', romaji: 'he', readingKo: '헤' },
        { kana: 'ほ', romaji: 'ho', readingKo: '호' },
      ],
      [
        { kana: 'ま', romaji: 'ma', readingKo: '마' },
        { kana: 'み', romaji: 'mi', readingKo: '미' },
        { kana: 'む', romaji: 'mu', readingKo: '무' },
        { kana: 'め', romaji: 'me', readingKo: '메' },
        { kana: 'も', romaji: 'mo', readingKo: '모' },
      ],
      [
        { kana: 'や', romaji: 'ya', readingKo: '야' },
        null,
        { kana: 'ゆ', romaji: 'yu', readingKo: '유' },
        null,
        { kana: 'よ', romaji: 'yo', readingKo: '요' },
      ],
      [
        { kana: 'ら', romaji: 'ra', readingKo: '라' },
        { kana: 'り', romaji: 'ri', readingKo: '리' },
        { kana: 'る', romaji: 'ru', readingKo: '루' },
        { kana: 'れ', romaji: 're', readingKo: '레' },
        { kana: 'ろ', romaji: 'ro', readingKo: '로' },
      ],
      [
        { kana: 'わ', romaji: 'wa', readingKo: '와' },
        null,
        null,
        null,
        { kana: 'を', romaji: 'wo', readingKo: '오' },
      ],
      [{ kana: 'ん', romaji: 'n', readingKo: '응' }, null, null, null, null],
    ],
  },
  {
    title: '탁음·반탁음',
    rows: [
      [
        { kana: 'が', romaji: 'ga', readingKo: '가' },
        { kana: 'ぎ', romaji: 'gi', readingKo: '기' },
        { kana: 'ぐ', romaji: 'gu', readingKo: '구' },
        { kana: 'げ', romaji: 'ge', readingKo: '게' },
        { kana: 'ご', romaji: 'go', readingKo: '고' },
      ],
      [
        { kana: 'ざ', romaji: 'za', readingKo: '자' },
        { kana: 'じ', romaji: 'ji', readingKo: '지' },
        { kana: 'ず', romaji: 'zu', readingKo: '즈' },
        { kana: 'ぜ', romaji: 'ze', readingKo: '제' },
        { kana: 'ぞ', romaji: 'zo', readingKo: '조' },
      ],
      [
        { kana: 'だ', romaji: 'da', readingKo: '다' },
        { kana: 'ぢ', romaji: 'ji', readingKo: '지' },
        { kana: 'づ', romaji: 'zu', readingKo: '즈' },
        { kana: 'で', romaji: 'de', readingKo: '데' },
        { kana: 'ど', romaji: 'do', readingKo: '도' },
      ],
      [
        { kana: 'ば', romaji: 'ba', readingKo: '바' },
        { kana: 'び', romaji: 'bi', readingKo: '비' },
        { kana: 'ぶ', romaji: 'bu', readingKo: '부' },
        { kana: 'べ', romaji: 'be', readingKo: '베' },
        { kana: 'ぼ', romaji: 'bo', readingKo: '보' },
      ],
      [
        { kana: 'ぱ', romaji: 'pa', readingKo: '파' },
        { kana: 'ぴ', romaji: 'pi', readingKo: '피' },
        { kana: 'ぷ', romaji: 'pu', readingKo: '푸' },
        { kana: 'ぺ', romaji: 'pe', readingKo: '페' },
        { kana: 'ぽ', romaji: 'po', readingKo: '포' },
      ],
    ],
  },
  {
    title: '요음',
    rows: [
      [
        { kana: 'きゃ', romaji: 'kya', readingKo: '캬' },
        { kana: 'きゅ', romaji: 'kyu', readingKo: '큐' },
        { kana: 'きょ', romaji: 'kyo', readingKo: '쿄' },
      ],
      [
        { kana: 'しゃ', romaji: 'sha', readingKo: '샤' },
        { kana: 'しゅ', romaji: 'shu', readingKo: '슈' },
        { kana: 'しょ', romaji: 'sho', readingKo: '쇼' },
      ],
      [
        { kana: 'ちゃ', romaji: 'cha', readingKo: '챠' },
        { kana: 'ちゅ', romaji: 'chu', readingKo: '츄' },
        { kana: 'ちょ', romaji: 'cho', readingKo: '쵸' },
      ],
      [
        { kana: 'にゃ', romaji: 'nya', readingKo: '냐' },
        { kana: 'にゅ', romaji: 'nyu', readingKo: '뉴' },
        { kana: 'にょ', romaji: 'nyo', readingKo: '뇨' },
      ],
      [
        { kana: 'ひゃ', romaji: 'hya', readingKo: '햐' },
        { kana: 'ひゅ', romaji: 'hyu', readingKo: '휴' },
        { kana: 'ひょ', romaji: 'hyo', readingKo: '효' },
      ],
      [
        { kana: 'みゃ', romaji: 'mya', readingKo: '먀' },
        { kana: 'みゅ', romaji: 'myu', readingKo: '뮤' },
        { kana: 'みょ', romaji: 'myo', readingKo: '묘' },
      ],
      [
        { kana: 'りゃ', romaji: 'rya', readingKo: '랴' },
        { kana: 'りゅ', romaji: 'ryu', readingKo: '류' },
        { kana: 'りょ', romaji: 'ryo', readingKo: '료' },
      ],
      [
        { kana: 'ぎゃ', romaji: 'gya', readingKo: '갸' },
        { kana: 'ぎゅ', romaji: 'gyu', readingKo: '규' },
        { kana: 'ぎょ', romaji: 'gyo', readingKo: '교' },
      ],
      [
        { kana: 'じゃ', romaji: 'ja', readingKo: '자' },
        { kana: 'じゅ', romaji: 'ju', readingKo: '주' },
        { kana: 'じょ', romaji: 'jo', readingKo: '조' },
      ],
      [
        { kana: 'びゃ', romaji: 'bya', readingKo: '뱌' },
        { kana: 'びゅ', romaji: 'byu', readingKo: '뷰' },
        { kana: 'びょ', romaji: 'byo', readingKo: '뵤' },
      ],
      [
        { kana: 'ぴゃ', romaji: 'pya', readingKo: '퍄' },
        { kana: 'ぴゅ', romaji: 'pyu', readingKo: '퓨' },
        { kana: 'ぴょ', romaji: 'pyo', readingKo: '표' },
      ],
    ],
  },
]

const wordCards = [
  { word: 'やま', readingKo: '야마', meaningKo: '산', romaji: 'yama', category: 'basic' },
  { word: 'いぬ', readingKo: '이누', meaningKo: '개', romaji: 'inu', category: 'basic' },
  { word: 'ねこ', readingKo: '네코', meaningKo: '고양이', romaji: 'neko', category: 'basic' },
  { word: 'みず', readingKo: '미즈', meaningKo: '물', romaji: 'mizu', category: 'basic' },
  { word: 'そら', readingKo: '소라', meaningKo: '하늘', romaji: 'sora', category: 'basic' },
  { word: 'はな', readingKo: '하나', meaningKo: '꽃', romaji: 'hana', category: 'basic' },
  { word: 'くも', readingKo: '쿠모', meaningKo: '구름', romaji: 'kumo', category: 'basic' },
  { word: 'あめ', readingKo: '아메', meaningKo: '비', romaji: 'ame', category: 'basic' },
  { word: 'うみ', readingKo: '우미', meaningKo: '바다', romaji: 'umi', category: 'basic' },
  { word: 'かさ', readingKo: '카사', meaningKo: '우산', romaji: 'kasa', category: 'basic' },
  { word: 'いえ', readingKo: '이에', meaningKo: '집', romaji: 'ie', category: 'basic' },
  { word: 'へや', readingKo: '헤야', meaningKo: '방', romaji: 'heya', category: 'basic' },
  { word: 'つくえ', readingKo: '츠쿠에', meaningKo: '책상', romaji: 'tsukue', category: 'basic' },
  { word: 'いす', readingKo: '이스', meaningKo: '의자', romaji: 'isu', category: 'basic' },
  { word: 'ほん', readingKo: '혼', meaningKo: '책', romaji: 'hon', category: 'basic' },
  { word: 'て', readingKo: '테', meaningKo: '손', romaji: 'te', category: 'basic' },
  { word: 'あし', readingKo: '아시', meaningKo: '발', romaji: 'ashi', category: 'basic' },
  { word: 'め', readingKo: '메', meaningKo: '눈', romaji: 'me', category: 'basic' },
  { word: 'みみ', readingKo: '미미', meaningKo: '귀', romaji: 'mimi', category: 'basic' },
  { word: 'くち', readingKo: '쿠치', meaningKo: '입', romaji: 'kuchi', category: 'basic' },
  { word: 'たべる', readingKo: '타베루', meaningKo: '먹다', romaji: 'taberu', category: 'basic' },
  { word: 'みる', readingKo: '미루', meaningKo: '보다', romaji: 'miru', category: 'basic' },
  { word: 'きく', readingKo: '키쿠', meaningKo: '듣다/묻다', romaji: 'kiku', category: 'basic' },
  { word: 'ねる', readingKo: '네루', meaningKo: '자다', romaji: 'neru', category: 'basic' },
  { word: 'あかい', readingKo: '아카이', meaningKo: '빨갛다', romaji: 'akai', category: 'basic' },
  { word: 'かぎ', readingKo: '카기', meaningKo: '열쇠', romaji: 'kagi', category: 'dakuon' },
  { word: 'ごはん', readingKo: '고한', meaningKo: '밥', romaji: 'gohan', category: 'dakuon' },
  { word: 'かぜ', readingKo: '카제', meaningKo: '바람/감기', romaji: 'kaze', category: 'dakuon' },
  { word: 'かぞく', readingKo: '카조쿠', meaningKo: '가족', romaji: 'kazoku', category: 'dakuon' },
  { word: 'ちず', readingKo: '치즈', meaningKo: '지도', romaji: 'chizu', category: 'dakuon' },
  { word: 'でんわ', readingKo: '덴와', meaningKo: '전화', romaji: 'denwa', category: 'dakuon' },
  { word: 'まど', readingKo: '마도', meaningKo: '창문', romaji: 'mado', category: 'dakuon' },
  { word: 'ぶた', readingKo: '부타', meaningKo: '돼지', romaji: 'buta', category: 'dakuon' },
  { word: 'かばん', readingKo: '카방', meaningKo: '가방', romaji: 'kaban', category: 'dakuon' },
  { word: 'えび', readingKo: '에비', meaningKo: '새우', romaji: 'ebi', category: 'dakuon' },
  { word: 'たべもの', readingKo: '타베모노', meaningKo: '음식', romaji: 'tabemono', category: 'dakuon' },
  { word: 'のぼる', readingKo: '노보루', meaningKo: '오르다', romaji: 'noboru', category: 'dakuon' },
  { word: 'あそぶ', readingKo: '아소부', meaningKo: '놀다', romaji: 'asobu', category: 'dakuon' },
  { word: 'げんき', readingKo: '겡키', meaningKo: '건강함/활기', romaji: 'genki', category: 'dakuon' },
  { word: 'だいじ', readingKo: '다이지', meaningKo: '중요함', romaji: 'daiji', category: 'dakuon' },
  { word: 'ぱん', readingKo: '판', meaningKo: '빵', romaji: 'pan', category: 'handakuon' },
  { word: 'ぴかぴか', readingKo: '피카피카', meaningKo: '반짝반짝', romaji: 'pikapika', category: 'handakuon' },
  { word: 'えんぴつ', readingKo: '엔피츠', meaningKo: '연필', romaji: 'enpitsu', category: 'handakuon' },
  { word: 'さんぽ', readingKo: '산포', meaningKo: '산책', romaji: 'sanpo', category: 'handakuon' },
  { word: 'きっぷ', readingKo: '킵푸', meaningKo: '표', romaji: 'kippu', category: 'handakuon' },
  { word: 'てんぷら', readingKo: '텐푸라', meaningKo: '튀김', romaji: 'tenpura', category: 'handakuon' },
  { word: 'ぽかぽか', readingKo: '포카포카', meaningKo: '따끈따끈', romaji: 'pokapoka', category: 'handakuon' },
  { word: 'ぺらぺら', readingKo: '페라페라', meaningKo: '술술 말함', romaji: 'perapera', category: 'handakuon' },
  { word: 'ぷるぷる', readingKo: '푸루푸루', meaningKo: '말랑말랑/떨림', romaji: 'purupuru', category: 'handakuon' },
  { word: 'しんぱい', readingKo: '신파이', meaningKo: '걱정', romaji: 'shinpai', category: 'handakuon' },
  { word: 'しゃしん', readingKo: '샤신', meaningKo: '사진', romaji: 'shashin', category: 'youon' },
  { word: 'きょう', readingKo: '쿄-', meaningKo: '오늘', romaji: 'kyou', category: 'youon' },
  { word: 'きゅうり', readingKo: '큐-리', meaningKo: '오이', romaji: 'kyuuri', category: 'youon' },
  { word: 'ぎゅうにゅう', readingKo: '규-뉴-', meaningKo: '우유', romaji: 'gyuunyuu', category: 'youon' },
  { word: 'じしょ', readingKo: '지쇼', meaningKo: '사전', romaji: 'jisho', category: 'youon' },
  { word: 'じゅぎょう', readingKo: '주교-', meaningKo: '수업', romaji: 'jugyou', category: 'youon' },
  { word: 'びょういん', readingKo: '뵤-인', meaningKo: '병원', romaji: 'byouin', category: 'youon' },
  { word: 'りょうり', readingKo: '료-리', meaningKo: '요리', romaji: 'ryouri', category: 'youon' },
  { word: 'しゅくだい', readingKo: '슈쿠다이', meaningKo: '숙제', romaji: 'shukudai', category: 'youon' },
  { word: 'おちゃ', readingKo: '오챠', meaningKo: '차', romaji: 'ocha', category: 'youon' },
  { word: 'ちゃいろ', readingKo: '챠이로', meaningKo: '갈색', romaji: 'chairo', category: 'youon' },
  { word: 'にゅうがく', readingKo: '뉴-가쿠', meaningKo: '입학', romaji: 'nyuugaku', category: 'youon' },
  { word: 'ひゃく', readingKo: '햐쿠', meaningKo: '백', romaji: 'hyaku', category: 'youon' },
  { word: 'みょうじ', readingKo: '묘-지', meaningKo: '성씨', romaji: 'myouji', category: 'youon' },
  { word: 'りょこう', readingKo: '료코-', meaningKo: '여행', romaji: 'ryokou', category: 'youon' },
  { word: 'きゃく', readingKo: '캬쿠', meaningKo: '손님', romaji: 'kyaku', category: 'youon' },
  { word: 'しょうゆ', readingKo: '쇼-유', meaningKo: '간장', romaji: 'shouyu', category: 'youon' },
  { word: 'ちゅうしゃ', readingKo: '츄-샤', meaningKo: '주사/주차', romaji: 'chuusha', category: 'youon' },
  { word: 'じゃま', readingKo: '자마', meaningKo: '방해', romaji: 'jama', category: 'youon' },
  { word: 'ぴょんぴょん', readingKo: '푠푠', meaningKo: '깡충깡충', romaji: 'pyonpyon', category: 'youon' },
  { word: 'がっこう', readingKo: '각코-', meaningKo: '학교', romaji: 'gakkou', category: 'sokuon' },
  { word: 'きって', readingKo: '킷테', meaningKo: '우표', romaji: 'kitte', category: 'sokuon' },
  { word: 'ざっし', readingKo: '잣시', meaningKo: '잡지', romaji: 'zasshi', category: 'sokuon' },
  { word: 'きっさてん', readingKo: '킷사텐', meaningKo: '찻집/카페', romaji: 'kissaten', category: 'sokuon' },
  { word: 'けっこん', readingKo: '켓콘', meaningKo: '결혼', romaji: 'kekkon', category: 'sokuon' },
  { word: 'いっぱい', readingKo: '입파이', meaningKo: '가득/한 잔', romaji: 'ippai', category: 'sokuon' },
  { word: 'ちょっと', readingKo: '춋토', meaningKo: '잠깐/조금', romaji: 'chotto', category: 'sokuon' },
  { word: 'もっと', readingKo: '못토', meaningKo: '더', romaji: 'motto', category: 'sokuon' },
  { word: 'ずっと', readingKo: '즛토', meaningKo: '계속', romaji: 'zutto', category: 'sokuon' },
  { word: 'ゆっくり', readingKo: '윳쿠리', meaningKo: '천천히', romaji: 'yukkuri', category: 'sokuon' },
  { word: 'あったかい', readingKo: '앗타카이', meaningKo: '따뜻하다', romaji: 'attakai', category: 'sokuon' },
  { word: 'まっすぐ', readingKo: '맛스구', meaningKo: '곧장', romaji: 'massugu', category: 'sokuon' },
  { word: 'しっぱい', readingKo: '십파이', meaningKo: '실패', romaji: 'shippai', category: 'sokuon' },
  { word: 'りっぱ', readingKo: '립파', meaningKo: '훌륭함', romaji: 'rippa', category: 'sokuon' },
  { word: 'はっぱ', readingKo: '핫파', meaningKo: '잎', romaji: 'happa', category: 'sokuon' },
  { word: 'おとうさん', readingKo: '오토-상', meaningKo: '아버지', romaji: 'otousan', category: 'chouon' },
  { word: 'おかあさん', readingKo: '오카-상', meaningKo: '어머니', romaji: 'okaasan', category: 'chouon' },
  { word: 'おにいさん', readingKo: '오니-상', meaningKo: '형/오빠', romaji: 'oniisan', category: 'chouon' },
  { word: 'おねえさん', readingKo: '오네-상', meaningKo: '누나/언니', romaji: 'oneesan', category: 'chouon' },
  { word: 'せんせい', readingKo: '센세-', meaningKo: '선생님', romaji: 'sensei', category: 'chouon' },
  { word: 'とけい', readingKo: '토케-', meaningKo: '시계', romaji: 'tokei', category: 'chouon' },
  { word: 'えいが', readingKo: '에-가', meaningKo: '영화', romaji: 'eiga', category: 'chouon' },
  { word: 'ゆうびん', readingKo: '유-빈', meaningKo: '우편', romaji: 'yuubin', category: 'chouon' },
  { word: 'くうき', readingKo: '쿠-키', meaningKo: '공기', romaji: 'kuuki', category: 'chouon' },
  { word: 'すうじ', readingKo: '스-지', meaningKo: '숫자', romaji: 'suuji', category: 'chouon' },
  { word: 'おおきい', readingKo: '오-키-', meaningKo: '크다', romaji: 'ookii', category: 'chouon' },
  { word: 'ちいさい', readingKo: '치-사이', meaningKo: '작다', romaji: 'chiisai', category: 'chouon' },
  { word: 'ありがとう', readingKo: '아리가토-', meaningKo: '고마워', romaji: 'arigatou', category: 'chouon' },
  { word: 'こうえん', readingKo: '코-엔', meaningKo: '공원', romaji: 'kouen', category: 'chouon' },
  { word: 'びょうき', readingKo: '뵤-키', meaningKo: '병', romaji: 'byouki', category: 'chouon' },
  { word: 'あさ', readingKo: '아사', meaningKo: '아침', romaji: 'asa', category: 'daily' },
  { word: 'ひる', readingKo: '히루', meaningKo: '낮/점심때', romaji: 'hiru', category: 'daily' },
  { word: 'よる', readingKo: '요루', meaningKo: '밤', romaji: 'yoru', category: 'daily' },
  { word: 'きのう', readingKo: '키노-', meaningKo: '어제', romaji: 'kinou', category: 'daily' },
  { word: 'あした', readingKo: '아시타', meaningKo: '내일', romaji: 'ashita', category: 'daily' },
  { word: 'いま', readingKo: '이마', meaningKo: '지금', romaji: 'ima', category: 'daily' },
  { word: 'まいにち', readingKo: '마이니치', meaningKo: '매일', romaji: 'mainichi', category: 'daily' },
  { word: 'じかん', readingKo: '지칸', meaningKo: '시간', romaji: 'jikan', category: 'daily' },
  { word: 'ごぜん', readingKo: '고젠', meaningKo: '오전', romaji: 'gozen', category: 'daily' },
  { word: 'ごご', readingKo: '고고', meaningKo: '오후', romaji: 'gogo', category: 'daily' },
  { word: 'ひと', readingKo: '히토', meaningKo: '사람', romaji: 'hito', category: 'daily' },
  { word: 'ともだち', readingKo: '토모다치', meaningKo: '친구', romaji: 'tomodachi', category: 'daily' },
  { word: 'こども', readingKo: '코도모', meaningKo: '아이', romaji: 'kodomo', category: 'daily' },
  { word: 'おとこ', readingKo: '오토코', meaningKo: '남자', romaji: 'otoko', category: 'daily' },
  { word: 'おんな', readingKo: '온나', meaningKo: '여자', romaji: 'onna', category: 'daily' },
  { word: 'なまえ', readingKo: '나마에', meaningKo: '이름', romaji: 'namae', category: 'daily' },
  { word: 'ことば', readingKo: '코토바', meaningKo: '말/언어', romaji: 'kotoba', category: 'daily' },
  { word: 'くに', readingKo: '쿠니', meaningKo: '나라', romaji: 'kuni', category: 'daily' },
  { word: 'まち', readingKo: '마치', meaningKo: '동네/거리', romaji: 'machi', category: 'daily' },
  { word: 'みち', readingKo: '미치', meaningKo: '길', romaji: 'michi', category: 'daily' },
  { word: 'えき', readingKo: '에키', meaningKo: '역', romaji: 'eki', category: 'daily' },
  { word: 'みせ', readingKo: '미세', meaningKo: '가게', romaji: 'mise', category: 'daily' },
  { word: 'かいしゃ', readingKo: '카이샤', meaningKo: '회사', romaji: 'kaisha', category: 'daily' },
  { word: 'しごと', readingKo: '시고토', meaningKo: '일/직업', romaji: 'shigoto', category: 'daily' },
  { word: 'やすみ', readingKo: '야스미', meaningKo: '휴식/휴일', romaji: 'yasumi', category: 'daily' },
  { word: 'くるま', readingKo: '쿠루마', meaningKo: '자동차', romaji: 'kuruma', category: 'daily' },
  { word: 'でんしゃ', readingKo: '덴샤', meaningKo: '전철/기차', romaji: 'densha', category: 'daily' },
  { word: 'おかね', readingKo: '오카네', meaningKo: '돈', romaji: 'okane', category: 'daily' },
  { word: 'くすり', readingKo: '쿠스리', meaningKo: '약', romaji: 'kusuri', category: 'daily' },
  { word: 'いしゃ', readingKo: '이샤', meaningKo: '의사', romaji: 'isha', category: 'daily' },
  { word: 'あさごはん', readingKo: '아사고한', meaningKo: '아침밥', romaji: 'asagohan', category: 'daily' },
  { word: 'ひるごはん', readingKo: '히루고한', meaningKo: '점심밥', romaji: 'hirugohan', category: 'daily' },
  { word: 'ばんごはん', readingKo: '방고한', meaningKo: '저녁밥', romaji: 'bangohan', category: 'daily' },
  { word: 'たまご', readingKo: '타마고', meaningKo: '달걀', romaji: 'tamago', category: 'daily' },
  { word: 'にく', readingKo: '니쿠', meaningKo: '고기', romaji: 'niku', category: 'daily' },
  { word: 'さかな', readingKo: '사카나', meaningKo: '생선', romaji: 'sakana', category: 'daily' },
  { word: 'やさい', readingKo: '야사이', meaningKo: '채소', romaji: 'yasai', category: 'daily' },
  { word: 'くだもの', readingKo: '쿠다모노', meaningKo: '과일', romaji: 'kudamono', category: 'daily' },
  { word: 'りんご', readingKo: '링고', meaningKo: '사과', romaji: 'ringo', category: 'daily' },
  { word: 'みかん', readingKo: '미칸', meaningKo: '귤', romaji: 'mikan', category: 'daily' },
  { word: 'かお', readingKo: '카오', meaningKo: '얼굴', romaji: 'kao', category: 'daily' },
  { word: 'あたま', readingKo: '아타마', meaningKo: '머리', romaji: 'atama', category: 'daily' },
  { word: 'からだ', readingKo: '카라다', meaningKo: '몸', romaji: 'karada', category: 'daily' },
  { word: 'こころ', readingKo: '코코로', meaningKo: '마음', romaji: 'kokoro', category: 'daily' },
  { word: 'ふく', readingKo: '후쿠', meaningKo: '옷', romaji: 'fuku', category: 'daily' },
  { word: 'くつ', readingKo: '쿠츠', meaningKo: '신발', romaji: 'kutsu', category: 'daily' },
  { word: 'べんきょう', readingKo: '벤쿄-', meaningKo: '공부', romaji: 'benkyou', category: 'daily' },
  { word: 'かう', readingKo: '카우', meaningKo: '사다', romaji: 'kau', category: 'daily' },
  { word: 'うる', readingKo: '우루', meaningKo: '팔다', romaji: 'uru', category: 'daily' },
  { word: 'いく', readingKo: '이쿠', meaningKo: '가다', romaji: 'iku', category: 'daily' },
  { word: 'くる', readingKo: '쿠루', meaningKo: '오다', romaji: 'kuru', category: 'daily' },
  { word: 'かえる', readingKo: '카에루', meaningKo: '돌아가다', romaji: 'kaeru', category: 'daily' },
  { word: 'はなす', readingKo: '하나스', meaningKo: '말하다', romaji: 'hanasu', category: 'daily' },
  { word: 'よむ', readingKo: '요무', meaningKo: '읽다', romaji: 'yomu', category: 'daily' },
  { word: 'かく', readingKo: '카쿠', meaningKo: '쓰다', romaji: 'kaku', category: 'daily' },
  { word: 'のむ', readingKo: '노무', meaningKo: '마시다', romaji: 'nomu', category: 'daily' },
  { word: 'おきる', readingKo: '오키루', meaningKo: '일어나다', romaji: 'okiru', category: 'daily' },
  { word: 'はたらく', readingKo: '하타라쿠', meaningKo: '일하다', romaji: 'hataraku', category: 'daily' },
  { word: 'やすむ', readingKo: '야스무', meaningKo: '쉬다', romaji: 'yasumu', category: 'daily' },
  { word: 'つくる', readingKo: '츠쿠루', meaningKo: '만들다', romaji: 'tsukuru', category: 'daily' },
  { word: 'つかう', readingKo: '츠카우', meaningKo: '사용하다', romaji: 'tsukau', category: 'daily' },
  { word: 'まつ', readingKo: '마츠', meaningKo: '기다리다', romaji: 'matsu', category: 'daily' },
  { word: 'あるく', readingKo: '아루쿠', meaningKo: '걷다', romaji: 'aruku', category: 'daily' },
  { word: 'はしる', readingKo: '하시루', meaningKo: '달리다', romaji: 'hashiru', category: 'daily' },
  { word: 'あつい', readingKo: '아츠이', meaningKo: '덥다/뜨겁다', romaji: 'atsui', category: 'daily' },
  { word: 'さむい', readingKo: '사무이', meaningKo: '춥다', romaji: 'samui', category: 'daily' },
  { word: 'あたらしい', readingKo: '아타라시-', meaningKo: '새롭다', romaji: 'atarashii', category: 'daily' },
  { word: 'ふるい', readingKo: '후루이', meaningKo: '오래되다', romaji: 'furui', category: 'daily' },
  { word: 'いい', readingKo: '이-', meaningKo: '좋다', romaji: 'ii', category: 'daily' },
  { word: 'わるい', readingKo: '와루이', meaningKo: '나쁘다', romaji: 'warui', category: 'daily' },
  { word: 'たかい', readingKo: '타카이', meaningKo: '비싸다/높다', romaji: 'takai', category: 'daily' },
  { word: 'やすい', readingKo: '야스이', meaningKo: '싸다', romaji: 'yasui', category: 'daily' },
  { word: 'おいしい', readingKo: '오이시-', meaningKo: '맛있다', romaji: 'oishii', category: 'daily' },
  { word: 'こんにちは', readingKo: '콘니치와', meaningKo: '안녕하세요', romaji: 'konnichiwa', category: 'daily' },
  { word: 'おはよう', readingKo: '오하요-', meaningKo: '좋은 아침', romaji: 'ohayou', category: 'daily' },
  { word: 'こんばんは', readingKo: '콘방와', meaningKo: '안녕하세요/좋은 저녁', romaji: 'konbanwa', category: 'daily' },
  { word: 'さようなら', readingKo: '사요-나라', meaningKo: '안녕히 가세요', romaji: 'sayounara', category: 'daily' },
  { word: 'すみません', readingKo: '스미마센', meaningKo: '실례합니다/죄송합니다', romaji: 'sumimasen', category: 'daily' },
]

const additionalDailyWords = [
  ['へや', '헤야', '방', 'heya'], ['げんかん', '겐칸', '현관', 'genkan'], ['まど', '마도', '창문', 'mado'], ['どあ', '도아', '문', 'doa'],
  ['ゆか', '유카', '바닥', 'yuka'], ['てんじょう', '텐죠-', '천장', 'tenjou'], ['かべ', '카베', '벽', 'kabe'], ['つくえ', '츠쿠에', '책상', 'tsukue'],
  ['いす', '이스', '의자', 'isu'], ['たな', '타나', '선반', 'tana'], ['べっど', '벳도', '침대', 'beddo'], ['ふとん', '후톤', '이불', 'futon'],
  ['まくら', '마쿠라', '베개', 'makura'], ['かがみ', '카가미', '거울', 'kagami'], ['かぎ', '카기', '열쇠', 'kagi'], ['でんき', '덴키', '전기/불', 'denki'],
  ['れいぞうこ', '레이조-코', '냉장고', 'reizouko'], ['せんたくき', '센타쿠키', '세탁기', 'sentakuki'], ['そうじき', '소-지키', '청소기', 'soujiki'], ['でんわ', '덴와', '전화', 'denwa'],
  ['てれび', '테레비', '텔레비전', 'terebi'], ['ぱそこん', '파소콘', '컴퓨터', 'pasokon'], ['ほんだな', '혼다나', '책장', 'hondana'], ['ごみばこ', '고미바코', '쓰레기통', 'gomibako'],
  ['たおる', '타오루', '수건', 'taoru'], ['せっけん', '셋켄', '비누', 'sekken'], ['はぶらし', '하부라시', '칫솔', 'haburashi'], ['はみがき', '하미가키', '양치', 'hamigaki'],
  ['しゃんぷー', '샴푸-', '샴푸', 'shanpuu'], ['かみ', '카미', '머리카락', 'kami'], ['め', '메', '눈', 'me'], ['みみ', '미미', '귀', 'mimi'],
  ['はな', '하나', '코', 'hana'], ['くち', '쿠치', '입', 'kuchi'], ['は', '하', '이', 'ha'], ['て', '테', '손', 'te'],
  ['あし', '아시', '발/다리', 'ashi'], ['ゆび', '유비', '손가락', 'yubi'], ['おなか', '오나카', '배', 'onaka'], ['せなか', '세나카', '등', 'senaka'],
  ['かた', '카타', '어깨', 'kata'], ['のど', '노도', '목', 'nodo'], ['びょうき', '뵤-키', '병', 'byouki'], ['けが', '케가', '부상', 'kega'],
  ['びょういん', '뵤-인', '병원', 'byouin'], ['よやく', '요야쿠', '예약', 'yoyaku'], ['けんこう', '켄코-', '건강', 'kenkou'], ['かぜ', '카제', '감기', 'kaze'],
  ['うわぎ', '우와기', '겉옷', 'uwagi'], ['したぎ', '시타기', '속옷', 'shitagi'], ['ぼうし', '보-시', '모자', 'boushi'], ['かばん', '카방', '가방', 'kaban'],
  ['さいふ', '사이후', '지갑', 'saifu'], ['めがね', '메가네', '안경', 'megane'], ['ゆびわ', '유비와', '반지', 'yubiwa'], ['かさ', '카사', '우산', 'kasa'],
  ['じてんしゃ', '지텐샤', '자전거', 'jitensha'], ['ばす', '바스', '버스', 'basu'], ['たくしー', '타쿠시-', '택시', 'takushii'], ['ひこうき', '히코-키', '비행기', 'hikouki'],
  ['くうこう', '쿠-코-', '공항', 'kuukou'], ['ちかてつ', '치카테츠', '지하철', 'chikatetsu'], ['しんごう', '싱고-', '신호등', 'shingou'], ['こうさてん', '코-사텐', '교차로', 'kousaten'],
  ['こうえん', '코-엔', '공원', 'kouen'], ['としょかん', '토쇼칸', '도서관', 'toshokan'], ['ぎんこう', '깅코-', '은행', 'ginkou'], ['ゆうびんきょく', '유-빙쿄쿠', '우체국', 'yuubinkyoku'],
  ['こんびに', '콘비니', '편의점', 'konbini'], ['すーぱー', '스-파-', '슈퍼마켓', 'suupaa'], ['れすとらん', '레스토랑', '식당', 'resutoran'], ['きっさてん', '킷사텐', '찻집', 'kissaten'],
  ['ほてる', '호테루', '호텔', 'hoteru'], ['がっこう', '각코-', '학교', 'gakkou'], ['きょうしつ', '쿄-시츠', '교실', 'kyoushitsu'], ['だいがく', '다이가쿠', '대학교', 'daigaku'],
  ['せんせい', '센세-', '선생님', 'sensei'], ['がくせい', '각세-', '학생', 'gakusei'], ['かぞく', '카조쿠', '가족', 'kazoku'], ['りょうしん', '료-신', '부모님', 'ryoushin'],
  ['ちち', '치치', '아버지', 'chichi'], ['はは', '하하', '어머니', 'haha'], ['あに', '아니', '형/오빠', 'ani'], ['あね', '아네', '누나/언니', 'ane'],
  ['おとうと', '오토-토', '남동생', 'otouto'], ['いもうと', '이모-토', '여동생', 'imouto'], ['しゅじん', '슈진', '남편', 'shujin'], ['つま', '츠마', '아내', 'tsuma'],
  ['みず', '미즈', '물', 'mizu'], ['おちゃ', '오차', '차', 'ocha'], ['こーひー', '코-히-', '커피', 'koohii'], ['ぎゅうにゅう', '규-뉴-', '우유', 'gyuunyuu'],
  ['じゅーす', '주-스', '주스', 'juusu'], ['ぱん', '팡', '빵', 'pan'], ['ごはん', '고한', '밥', 'gohan'], ['すーぷ', '스-푸', '수프', 'suupu'],
  ['さらだ', '사라다', '샐러드', 'sarada'], ['みそしる', '미소시루', '된장국', 'misoshiru'], ['うどん', '우동', '우동', 'udon'], ['そば', '소바', '메밀국수', 'soba'],
  ['らーめん', '라-멘', '라멘', 'raamen'], ['すし', '스시', '초밥', 'sushi'], ['おにぎり', '오니기리', '주먹밥', 'onigiri'], ['べんとう', '벤토-', '도시락', 'bentou'],
  ['しお', '시오', '소금', 'shio'], ['さとう', '사토-', '설탕', 'satou'], ['しょうゆ', '쇼-유', '간장', 'shouyu'], ['あぶら', '아부라', '기름', 'abura'],
  ['いぬ', '이누', '개', 'inu'], ['ねこ', '네코', '고양이', 'neko'], ['とり', '토리', '새', 'tori'], ['うま', '우마', '말', 'uma'],
  ['さくら', '사쿠라', '벚꽃', 'sakura'], ['はなび', '하나비', '불꽃놀이', 'hanabi'], ['あめ', '아메', '비', 'ame'], ['ゆき', '유키', '눈', 'yuki'],
  ['くも', '쿠모', '구름', 'kumo'], ['そら', '소라', '하늘', 'sora'], ['ほし', '호시', '별', 'hoshi'], ['つき', '츠키', '달', 'tsuki'],
  ['はる', '하루', '봄', 'haru'], ['なつ', '나츠', '여름', 'natsu'], ['あき', '아키', '가을', 'aki'], ['ふゆ', '후유', '겨울', 'fuyu'],
  ['けさ', '케사', '오늘 아침', 'kesa'], ['こんしゅう', '콘슈-', '이번 주', 'konshuu'], ['らいしゅう', '라이슈-', '다음 주', 'raishuu'], ['せんしゅう', '센슈-', '지난주', 'senshuu'],
  ['たんじょうび', '탄죠-비', '생일', 'tanjoubi'], ['しゅくだい', '슈쿠다이', '숙제', 'shukudai'], ['しつもん', '시츠몬', '질문', 'shitsumon'], ['こたえ', '코타에', '대답', 'kotae'],
  ['いみ', '이미', '의미', 'imi'], ['もんだい', '몬다이', '문제', 'mondai'], ['れんしゅう', '렌슈-', '연습', 'renshuu'], ['しけん', '시켄', '시험', 'shiken'],
  ['しゃしん', '샤신', '사진', 'shashin'], ['えいが', '에-가', '영화', 'eiga'], ['おんがく', '온가쿠', '음악', 'ongaku'], ['うた', '우타', '노래', 'uta'],
  ['にゅーす', '뉴-스', '뉴스', 'nyuusu'], ['げーむ', '게-무', '게임', 'geemu'], ['りょこう', '료코-', '여행', 'ryokou'], ['しゅみ', '슈미', '취미', 'shumi'],
].map(([word, readingKo, meaningKo, romaji]) => ({ word, readingKo, meaningKo, romaji, category: 'daily' }))

const ichidanVerbStems = [
  ['あけ', '아케', '열다', 'ake'], ['あげ', '아게', '주다', 'age'], ['あび', '아비', '샤워하다', 'abi'], ['あつめ', '아츠메', '모으다', 'atsume'],
  ['いれ', '이레', '넣다', 'ire'], ['うけ', '우케', '받다', 'uke'], ['うまれ', '우마레', '태어나다', 'umare'], ['おしえ', '오시에', '가르치다', 'oshie'],
  ['おり', '오리', '내리다', 'ori'], ['かけ', '카케', '걸다', 'kake'], ['かり', '카리', '빌리다', 'kari'], ['きめ', '키메', '정하다', 'kime'],
  ['こたえ', '코타에', '대답하다', 'kotae'], ['こわれ', '코와레', '고장 나다', 'koware'], ['しらべ', '시라베', '조사하다', 'shirabe'], ['しんじ', '신지', '믿다', 'shinji'],
  ['すて', '스테', '버리다', 'sute'], ['たすけ', '타스케', '돕다', 'tasuke'], ['たて', '타테', '세우다', 'tate'], ['つけ', '츠케', '켜다/붙이다', 'tsuke'],
  ['つづけ', '츠즈케', '계속하다', 'tsuzuke'], ['つとめ', '츠토메', '근무하다', 'tsutome'], ['でかけ', '데카케', '외출하다', 'dekake'], ['とめ', '토메', '멈추다', 'tome'],
  ['ならべ', '나라베', '늘어놓다', 'narabe'], ['にげ', '니게', '도망가다', 'nige'], ['はじめ', '하지메', '시작하다', 'hajime'], ['ほめ', '호메', '칭찬하다', 'home'],
  ['まけ', '마케', '지다', 'make'], ['まちがえ', '마치가에', '틀리다', 'machigae'], ['みせ', '미세', '보여주다', 'mise'], ['みつけ', '미츠케', '찾다', 'mitsuke'],
  ['むかえ', '무카에', '맞이하다', 'mukae'], ['やめ', '야메', '그만두다', 'yame'], ['わすれ', '와스레', '잊다', 'wasure'], ['わけ', '와케', '나누다', 'wake'],
  ['あきらめ', '아키라메', '포기하다', 'akirame'], ['おぼえ', '오보에', '외우다', 'oboe'], ['かんがえ', '캉가에', '생각하다', 'kangae'], ['くらべ', '쿠라베', '비교하다', 'kurabe'],
  ['しめ', '시메', '닫다', 'shime'], ['たしかめ', '타시카메', '확인하다', 'tashikame'], ['たのしめ', '타노시메', '즐기다', 'tanoshime'], ['つかれ', '츠카레', '피곤해지다', 'tsukare'],
  ['なれ', '나레', '익숙해지다', 'nare'], ['に', '니', '삶다', 'ni'], ['みとめ', '미토메', '인정하다', 'mitome'], ['あずけ', '아즈케', '맡기다', 'azuke'],
]

const additionalVerbCards = ichidanVerbStems.flatMap(([stem, readingStem, meaningKo, romajiStem]) => [
  { word: `${stem}る`, readingKo: `${readingStem}루`, meaningKo, romaji: `${romajiStem}ru`, category: 'daily' },
  { word: `${stem}ます`, readingKo: `${readingStem}마스`, meaningKo: `${meaningKo} (공손형)`, romaji: `${romajiStem}masu`, category: 'daily' },
  { word: `${stem}て`, readingKo: `${readingStem}테`, meaningKo: `${meaningKo} (연결형)`, romaji: `${romajiStem}te`, category: 'daily' },
  { word: `${stem}ない`, readingKo: `${readingStem}나이`, meaningKo: `${meaningKo} (부정형)`, romaji: `${romajiStem}nai`, category: 'daily' },
])

const finalDailyWords = [
  ['けいたい', '케-타이', '휴대전화', 'keitai'], ['じゅうでんき', '주-덴키', '충전기', 'juudenki'], ['いんたーねっと', '인타-넷토', '인터넷', 'intaanetto'], ['めーる', '메-루', '이메일', 'meeru'],
  ['かいぎ', '카이기', '회의', 'kaigi'], ['しりょう', '시료-', '자료', 'shiryou'], ['めも', '메모', '메모', 'memo'],
  ['いんさつ', '인사츠', '인쇄', 'insatsu'], ['でんたく', '덴타쿠', '계산기', 'dentaku'], ['れじ', '레지', '계산대', 'reji'], ['りょうしゅうしょ', '료-슈-쇼', '영수증', 'ryoushuusho'],
  ['ねだん', '네단', '가격', 'nedan'], ['おつり', '오츠리', '거스름돈', 'otsuri'], ['げんきん', '겐킨', '현금', 'genkin'], ['てぶくろ', '테부쿠로', '장갑', 'tebukuro'],
  ['ふでばこ', '후데바코', '필통', 'fudebako'], ['けしごむ', '케시고무', '지우개', 'keshigomu'], ['えんぴつ', '엔피츠', '연필', 'enpitsu'], ['のーと', '노-토', '노트', 'nooto'],
  ['じしょ', '지쇼', '사전', 'jisho'], ['ちず', '치즈', '지도', 'chizu'], ['かれんだー', '카렌다-', '달력', 'karendaa'], ['すいぞくかん', '스이조쿠칸', '수족관', 'suizokukan'],
  ['どうぶつえん', '도-부츠엔', '동물원', 'doubutsuen'], ['ぷれぜんと', '푸레젠토', '선물', 'purezento'], ['おまつり', '오마츠리', '축제', 'omatsuri'],
].map(([word, readingKo, meaningKo, romaji]) => ({ word, readingKo, meaningKo, romaji, category: 'daily' }))

const uniqueWordCards = Array.from(
  new Map([...wordCards, ...additionalDailyWords, ...additionalVerbCards, ...finalDailyWords].map((card) => [card.word, card])).values()
)

wordCards.splice(0, wordCards.length, ...uniqueWordCards)

const katakanaKanaCards = kanaCards.map(convertCardToKatakana)
const katakanaWordCards = wordCards.map(convertCardToKatakana)
const katakanaChartSections = convertChartSectionsToKatakana(hiraganaChartSections)
const combinedChartSections = hiraganaChartSections.map((section, sectionIndex) => ({
  ...section,
  rows: section.rows.map((row, rowIndex) =>
    row.map((item, itemIndex) => {
      if (!item) return null

      return {
        ...item,
        katakana: katakanaChartSections[sectionIndex].rows[rowIndex][itemIndex].kana,
      }
    })
  ),
}))

const level = ref(1)
const currentCard = ref(null)
const isAnswerVisible = ref(false)
const isAnswered = ref(false)
const isStarted = ref(false)
const questionStartedAt = ref(null)
const scriptMode = ref('hiragana')
const currentView = ref('trainer')
const recordMetric = ref('accuracy')
const recordSortDirection = ref('desc')
const reviewMode = ref(false)
const message = ref('')
const resultState = ref('')
const answerInput = ref('')
const answerInputElement = ref(null)
const isChartVisible = ref(false)
const isVocabularyVisible = ref(false)
const chartView = ref('hiragana')
const chartZoom = ref(1)
const vocabularyQuery = ref('')
const vocabularyVisibleColumns = ref({
  hiragana: true,
  katakana: true,
  reading: true,
  meaning: true,
})
const todayCount = ref(0)
const correctCount = ref(0)
const wrongCount = ref(0)
const wrongCards = ref({
  hiragana: [],
  katakana: [],
})
const cardStats = ref({})

const activeCards = computed(() => {
  if (reviewMode.value) {
    return currentWrongCards.value
  }

  return level.value === 1 ? trainingKanaCards.value : trainingWordCards.value
})

const cardText = computed(() => {
  if (!currentCard.value) return ''
  return currentCard.value.kana || currentCard.value.word
})

const scriptLabel = computed(() => (scriptMode.value === 'hiragana' ? '히라가나' : '가타카나'))

const chartEyebrow = computed(() => {
  if (chartView.value === 'combined') return 'Kana Chart'
  return chartView.value === 'hiragana' ? 'Hiragana Chart' : 'Katakana Chart'
})

const chartButtonLabel = computed(() => `${scriptLabel.value} 보기`)

const chartTitle = computed(() => {
  if (chartView.value === 'combined') return '히라가나 · 가타카나 전체 보기'
  return `${chartView.value === 'hiragana' ? '히라가나' : '가타카나'} 전체 보기`
})

const currentChartSections = computed(() =>
  chartView.value === 'katakana' ? katakanaChartSections : hiraganaChartSections
)

const currentWrongCards = computed(() => wrongCards.value[scriptMode.value])

const trainingKanaCards = computed(() => (scriptMode.value === 'hiragana' ? kanaCards : katakanaKanaCards))

const trainingWordCards = computed(() => (scriptMode.value === 'hiragana' ? wordCards : katakanaWordCards))

const levelLabel = computed(() => (level.value === 1 ? `${scriptLabel.value} 음 카드` : `${scriptLabel.value} 단어 카드`))

const recordCards = computed(() => (level.value === 1 ? trainingKanaCards.value : trainingWordCards.value))

const vocabularyCards = computed(() => {
  const query = vocabularyQuery.value.trim().toLowerCase()

  if (!query) return wordCards

  return wordCards.filter((card) =>
    [card.word, toKatakanaText(card.word), card.readingKo, card.meaningKo, card.romaji].some((value) =>
      value.toLowerCase().includes(query)
    )
  )
})

const visibleVocabularyColumnCount = computed(
  () => Object.values(vocabularyVisibleColumns.value).filter(Boolean).length
)

const recordSortLabel = computed(() => (recordSortDirection.value === 'desc' ? '내림차순' : '오름차순'))

const chartZoomPercent = computed(() => Math.round(chartZoom.value * 100))

const cardStatRows = computed(() => {
  return recordCards.value
    .map((card) => {
      const stats = cardStats.value[getStatKey(card)] || {
        correct: 0,
        wrong: 0,
        totalResponseMs: 0,
      }
      const total = stats.correct + stats.wrong
      const averageResponseMs =
        stats.correct === 0 ? null : Math.round(stats.totalResponseMs / stats.correct)

      return {
        text: getCardKey(card),
        readingKo: card.readingKo,
        meaningKo: card.meaningKo,
        correct: stats.correct,
        wrong: stats.wrong,
        total,
        accuracy: total === 0 ? null : Math.round((stats.correct / total) * 100),
        averageResponseMs,
      }
    })
    .filter((row) => row.total > 0)
    .sort((a, b) => {
      const direction = recordSortDirection.value === 'desc' ? -1 : 1

      if (recordMetric.value === 'speed') {
        if (a.averageResponseMs === null) return 1
        if (b.averageResponseMs === null) return -1
        return (
          (a.averageResponseMs - b.averageResponseMs) * direction ||
          b.total - a.total ||
          a.text.localeCompare(b.text, 'ja')
        )
      }

      return (a.accuracy - b.accuracy) * direction || b.total - a.total || a.text.localeCompare(b.text, 'ja')
    })
})

function getCardKey(card) {
  return card.kana || card.word
}

function toKatakanaText(text) {
  return text.replace(/[ぁ-ゖ]/g, (character) =>
    String.fromCharCode(character.charCodeAt(0) + 0x60)
  )
}

function convertCardToKatakana(card) {
  if (card.kana) {
    return {
      ...card,
      kana: toKatakanaText(card.kana),
    }
  }

  return {
    ...card,
    word: toKatakanaText(card.word),
  }
}

function convertChartSectionsToKatakana(sections) {
  return sections.map((section) => ({
    ...section,
    rows: section.rows.map((row) =>
      row.map((item) =>
        item
          ? {
              ...item,
              kana: toKatakanaText(item.kana),
            }
          : null
      )
    ),
  }))
}

function getStatKey(card) {
  return card.kana ? `kana:${card.kana}` : `word:${card.word}`
}

function getElapsedResponseMs() {
  if (!questionStartedAt.value) return 0

  return Date.now() - questionStartedAt.value
}

function formatResponseTime(milliseconds) {
  if (milliseconds === null) return '-'

  return `${(milliseconds / 1000).toFixed(2)}초`
}

function selectRecordMetric(nextMetric) {
  if (recordMetric.value === nextMetric) {
    recordSortDirection.value = recordSortDirection.value === 'desc' ? 'asc' : 'desc'
    return
  }

  recordMetric.value = nextMetric
  recordSortDirection.value = 'desc'
}

function changeChartZoom(amount) {
  chartZoom.value = Math.min(1.8, Math.max(0.85, Number((chartZoom.value + amount).toFixed(2))))
}

function resetChartZoom() {
  chartZoom.value = 1
}

function openChart() {
  chartView.value = scriptMode.value
  isChartVisible.value = true
}

function openVocabulary() {
  vocabularyQuery.value = ''
  isVocabularyVisible.value = true
}

function toggleVocabularyColumn(column) {
  if (vocabularyVisibleColumns.value[column] && visibleVocabularyColumnCount.value === 1) return
  vocabularyVisibleColumns.value[column] = !vocabularyVisibleColumns.value[column]
}

function recordCardResult(card, result, responseMs = 0) {
  const key = getStatKey(card)
  const currentStats = cardStats.value[key] || {
    correct: 0,
    wrong: 0,
    totalResponseMs: 0,
  }

  cardStats.value[key] = {
    ...currentStats,
    [result]: currentStats[result] + 1,
    totalResponseMs:
      result === 'correct' ? currentStats.totalResponseMs + responseMs : currentStats.totalResponseMs,
  }
}

function normalizeAnswer(answer) {
  return answer
    .trim()
    .toLowerCase()
    .replace(/[－ー―–—]/g, '-')
    .replace(/\s+/g, '')
}

function getLongVowelOptions(syllable) {
  const code = syllable.charCodeAt(0) - 0xac00

  if (code < 0 || code > 11171) return ['']

  const vowelIndex = Math.floor((code % 588) / 28)
  const vowelOptions = {
    0: ['아'],
    2: ['야'],
    8: ['우', '오'],
    12: ['우', '오'],
    13: ['우'],
    17: ['우'],
    18: ['이'],
    20: ['이', '에'],
  }

  return vowelOptions[vowelIndex] || ['']
}

function addLongVowelVariants(answers, answer) {
  if (!answer.includes('-')) return

  const variants = ['']

  for (let index = 0; index < answer.length; index += 1) {
    const character = answer[index]

    if (character !== '-') {
      for (let variantIndex = 0; variantIndex < variants.length; variantIndex += 1) {
        variants[variantIndex] += character
      }
      continue
    }

    const previousCharacter = answer[index - 1]
    const options = getLongVowelOptions(previousCharacter)
    const expandedVariants = []

    variants.forEach((variant) => {
      options.forEach((option) => expandedVariants.push(`${variant}${option}`))
    })

    variants.splice(0, variants.length, ...expandedVariants)
  }

  variants.forEach((variant) => answers.add(variant))
}

function addReadingVariants(answers, answer) {
  const variantGroups = [
    ['챠', '차'],
    ['츄', '추'],
    ['쵸', '초'],
    ['쟈', '자'],
    ['쥬', '주'],
    ['죠', '조'],
    ['샤', '샤'],
    ['슈', '슈'],
    ['쇼', '쇼'],
    ['캬', '캬'],
    ['큐', '큐'],
    ['쿄', '쿄'],
    ['갸', '갸'],
    ['규', '규'],
    ['교', '교'],
  ]

  variantGroups.forEach(([from, to]) => {
    if (answer.includes(from)) answers.add(answer.replaceAll(from, to))
    if (answer.includes(to)) answers.add(answer.replaceAll(to, from))
  })

  if (answer.includes('-')) {
    answers.add(answer.replaceAll('-', ''))
    addLongVowelVariants(answers, answer)
  }

  if (answer.includes('각코')) {
    answers.add(answer.replace('각코', '갓코'))
  }

  if (answer.includes('갓코')) {
    answers.add(answer.replace('갓코', '각코'))
  }
}

function getAcceptedAnswers(card) {
  const answers = new Set([card.readingKo])

  if (card.answersKo) {
    card.answersKo.forEach((answer) => answers.add(answer))
  }

  Array.from(answers).forEach((answer) => addReadingVariants(answers, answer))

  return Array.from(answers).map(normalizeAnswer)
}

function isCorrectAnswer(card, answer) {
  if (!card || !answer.trim()) return false

  return getAcceptedAnswers(card).includes(normalizeAnswer(answer))
}

function countWrong(card, timeoutMessage) {
  todayCount.value += 1
  wrongCount.value += 1
  recordCardResult(card, 'wrong')
  questionStartedAt.value = null
  isAnswered.value = true
  resultState.value = 'wrong'

  if (!currentWrongCards.value.some((wrongCard) => getCardKey(wrongCard) === getCardKey(card))) {
    currentWrongCards.value.push(card)
  }

  message.value = timeoutMessage || '틀린 카드에 저장했습니다'
}

// 현재 카드와 바로 같은 카드가 다시 나오지 않도록 랜덤 카드를 고른다.
function pickRandomCard(cards) {
  if (cards.length === 0) return null
  if (cards.length === 1) return cards[0]

  let nextCard = cards[Math.floor(Math.random() * cards.length)]

  while (currentCard.value && getCardKey(nextCard) === getCardKey(currentCard.value)) {
    nextCard = cards[Math.floor(Math.random() * cards.length)]
  }

  return nextCard
}

function giveUpAndShowAnswer() {
  if (isAnswered.value || !currentCard.value) return

  isAnswerVisible.value = true
  countWrong(currentCard.value, `정답은 ${currentCard.value.readingKo}입니다`)
}

function focusAnswerInput() {
  nextTick(() => {
    answerInputElement.value?.focus({ preventScroll: true })
  })
}

function handleTrainerEnter(event) {
  if (event.key !== 'Enter' || event.isComposing) return
  if (currentView.value !== 'trainer' || !isAnswered.value) return

  event.preventDefault()
  nextCard()
}

function submitAnswer() {
  if (isAnswered.value) {
    nextCard()
    return
  }

  if (!currentCard.value) return

  if (!answerInput.value.trim()) {
    message.value = '답을 입력해 주세요'
    resultState.value = ''
    return
  }

  isAnswerVisible.value = true

  if (isCorrectAnswer(currentCard.value, answerInput.value)) {
    markCorrect(getElapsedResponseMs())
    return
  }

  countWrong(currentCard.value, `오답입니다. 정답은 ${currentCard.value.readingKo}입니다`)
}

function startTraining() {
  reviewMode.value = false
  isStarted.value = true
  nextCard()
}

function nextCard() {
  const cards = activeCards.value

  if (reviewMode.value && cards.length === 0) {
    message.value = '틀린 카드가 없습니다'
    reviewMode.value = false
    isStarted.value = false
    currentCard.value = null
    questionStartedAt.value = null
    return
  }

  currentCard.value = pickRandomCard(cards)
  isAnswerVisible.value = false
  isAnswered.value = false
  answerInput.value = ''
  questionStartedAt.value = Date.now()
  message.value = reviewMode.value ? '틀린 카드 복습 중입니다' : ''
  resultState.value = ''
  focusAnswerInput()
}

function markCorrect(responseMs = getElapsedResponseMs()) {
  if (isAnswered.value || !currentCard.value) return

  todayCount.value += 1
  correctCount.value += 1
  recordCardResult(currentCard.value, 'correct', responseMs)
  questionStartedAt.value = null
  isAnswered.value = true
  resultState.value = 'correct'
  message.value = `정답입니다 · ${formatResponseTime(responseMs)}`
}

function startWrongReview() {
  if (currentWrongCards.value.length === 0) {
    message.value = '틀린 카드가 없습니다'
    return
  }

  reviewMode.value = true
  isStarted.value = true
  nextCard()
}

function returnToTraining() {
  reviewMode.value = false
  isStarted.value = true
  nextCard()
}

function resetTrainingState() {
  reviewMode.value = false
  isStarted.value = false
  currentCard.value = null
  isAnswerVisible.value = false
  isAnswered.value = false
  answerInput.value = ''
  resultState.value = ''
  message.value = ''
  questionStartedAt.value = null
}

function changeScriptMode(nextMode) {
  if (scriptMode.value === nextMode) return

  scriptMode.value = nextMode
  resetTrainingState()
}

function changeLevel(nextLevel) {
  level.value = nextLevel
}

watch(level, () => {
  resetTrainingState()
})

onMounted(() => {
  window.addEventListener('keydown', handleTrainerEnter)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleTrainerEnter)
})
</script>

<template>
  <main class="app-shell">
    <section class="trainer">
      <header class="header">
        <div class="top-bar">
          <p class="eyebrow">Japanese Reading</p>
          <div class="top-actions">
            <button class="vocabulary-open-button" type="button" @click="openVocabulary">단어장</button>
            <button class="chart-open-button" type="button" @click="openChart">{{ chartButtonLabel }}</button>
          </div>
        </div>
        <div class="title-row">
          <h1>일본어 읽기 훈련</h1>
          <div class="script-tabs" aria-label="문자 선택">
            <button
              :class="{ active: scriptMode === 'hiragana' }"
              type="button"
              @click="changeScriptMode('hiragana')"
            >
              히라가나
            </button>
            <button
              :class="{ active: scriptMode === 'katakana' }"
              type="button"
              @click="changeScriptMode('katakana')"
            >
              가타카나
            </button>
          </div>
        </div>
      </header>

      <div class="view-tabs" aria-label="화면 선택">
        <button :class="{ active: currentView === 'trainer' }" type="button" @click="currentView = 'trainer'">
          훈련
        </button>
        <button :class="{ active: currentView === 'stats' }" type="button" @click="currentView = 'stats'">
          기록
        </button>
      </div>

      <template v-if="currentView === 'trainer'">
        <div class="level-tabs" aria-label="난이도 선택">
          <button :class="{ active: level === 1 }" type="button" @click="changeLevel(1)">
          난이도 1
          </button>
          <button :class="{ active: level === 2 }" type="button" @click="changeLevel(2)">
          난이도 2
          </button>
        </div>

        <p class="level-label">{{ levelLabel }}</p>

        <article class="card" :class="resultState">
          <template v-if="!isStarted">
            <p class="ready-title">준비되면 시작하세요</p>
            <p class="ready-copy">
              답을 제출할 때까지 걸린 시간을 함께 기록합니다
            </p>
          </template>

          <template v-else>
            <p class="review-badge" v-if="reviewMode">틀린 카드 복습</p>
            <div class="prompt">{{ cardText }}</div>
          </template>

          <div class="answer" v-if="isAnswerVisible && currentCard">
            <p><span>발음</span>{{ currentCard.readingKo }}</p>
            <p><span>로마자</span>{{ currentCard.romaji }}</p>
            <p v-if="currentCard.meaningKo"><span>뜻</span>{{ currentCard.meaningKo }}</p>
          </div>

          <p class="message" :class="resultState" v-if="message">{{ message }}</p>
        </article>

        <form class="answer-form" v-if="isStarted" @submit.prevent="submitAnswer">
          <input
            ref="answerInputElement"
            v-model="answerInput"
            type="text"
            autocomplete="off"
            autocapitalize="off"
            placeholder="한글 발음을 입력하세요. 예: 오토-상"
            aria-label="정답 입력"
          />
          <button type="submit">{{ isAnswered ? '다음' : '전송' }}</button>
        </form>

        <div class="actions">
          <button class="primary-action" v-if="!isStarted" type="button" @click="startTraining">시작</button>
          <button class="primary-action muted-action" v-else-if="!isAnswered" type="button" @click="giveUpAndShowAnswer">정답 보기</button>
          <button class="wide" v-if="reviewMode" type="button" @click="returnToTraining">
            일반 훈련으로 돌아가기
          </button>
          <button class="wide" v-else type="button" @click="startWrongReview">틀린 카드 복습</button>
        </div>

        <dl class="stats">
          <div>
            <dt>오늘 푼 개수</dt>
            <dd>{{ todayCount }}</dd>
          </div>
          <div>
            <dt>맞은 개수</dt>
            <dd>{{ correctCount }}</dd>
          </div>
          <div>
            <dt>틀린 개수</dt>
            <dd>{{ wrongCount }}</dd>
          </div>
        </dl>
      </template>

      <section v-else class="record-panel">
        <dl class="stats record-stats">
          <div>
            <dt>현재까지 푼 문제</dt>
            <dd>{{ todayCount }}</dd>
          </div>
          <div>
            <dt>정답 개수</dt>
            <dd>{{ correctCount }}</dd>
          </div>
          <div>
            <dt>오답 개수</dt>
            <dd>{{ wrongCount }}</dd>
          </div>
        </dl>

        <article class="record-card">
          <header class="record-header">
            <div>
              <p class="level-label">난이도 {{ level }}</p>
              <h2>
                {{
                  recordMetric === 'accuracy'
                    ? level === 1
                      ? '음절별 정답률'
                      : '단어별 정답률'
                    : level === 1
                      ? '음절별 응답속도'
                      : '단어별 응답속도'
                }}
              </h2>
            </div>
          </header>

          <div class="metric-tabs" aria-label="기록 기준 선택">
            <button
              :class="{ active: recordMetric === 'accuracy' }"
              type="button"
              @click="selectRecordMetric('accuracy')"
            >
              정답률
              <span v-if="recordMetric === 'accuracy'">{{ recordSortLabel }}</span>
            </button>
            <button
              :class="{ active: recordMetric === 'speed' }"
              type="button"
              @click="selectRecordMetric('speed')"
            >
              응답속도
              <span v-if="recordMetric === 'speed'">{{ recordSortLabel }}</span>
            </button>
          </div>

          <p class="empty-record" v-if="cardStatRows.length === 0">
            아직 기록된 카드가 없습니다
          </p>

          <div class="record-list" v-else>
            <div class="record-row" v-for="row in cardStatRows" :key="row.text">
              <div class="record-name">
                <strong>{{ row.text }}</strong>
                <span>{{ row.readingKo }}<template v-if="row.meaningKo"> · {{ row.meaningKo }}</template></span>
              </div>
              <div class="record-counts">
                <span>정답 {{ row.correct }}</span>
                <span>오답 {{ row.wrong }}</span>
                <span v-if="recordMetric === 'speed'">정답 평균 {{ formatResponseTime(row.averageResponseMs) }}</span>
              </div>
              <strong class="record-rate">
                {{ recordMetric === 'accuracy' ? `${row.accuracy}%` : formatResponseTime(row.averageResponseMs) }}
              </strong>
            </div>
          </div>
        </article>
      </section>
    </section>

    <div class="chart-overlay" v-if="isChartVisible" @click.self="isChartVisible = false">
      <section class="chart-dialog" :aria-label="`${scriptLabel} 전체 표`">
        <header class="chart-header">
          <div>
            <p class="eyebrow">{{ chartEyebrow }}</p>
            <h2>{{ chartTitle }}</h2>
          </div>
          <button class="chart-close-button" type="button" aria-label="닫기" @click="isChartVisible = false">
            닫기
          </button>
        </header>

        <div class="chart-mode-tabs" aria-label="전체 보기 문자 선택">
          <button
            type="button"
            :class="{ active: chartView === 'hiragana' }"
            :aria-pressed="chartView === 'hiragana'"
            @click="chartView = 'hiragana'"
          >
            히라가나
          </button>
          <button
            type="button"
            :class="{ active: chartView === 'katakana' }"
            :aria-pressed="chartView === 'katakana'"
            @click="chartView = 'katakana'"
          >
            가타카나
          </button>
          <button
            type="button"
            :class="{ active: chartView === 'combined' }"
            :aria-pressed="chartView === 'combined'"
            @click="chartView = 'combined'"
          >
            같이 보기
          </button>
        </div>

        <div class="chart-controls" aria-label="표 확대 조절">
          <button type="button" @click="changeChartZoom(-0.1)">축소</button>
          <span>{{ chartZoomPercent }}%</span>
          <button type="button" @click="changeChartZoom(0.1)">확대</button>
          <button type="button" @click="resetChartZoom">초기화</button>
        </div>

        <div class="chart-scroll" :class="{ combined: chartView === 'combined' }" :style="{ '--chart-zoom': chartZoom }">
          <template v-if="chartView !== 'combined'">
            <section class="chart-section" v-for="section in currentChartSections" :key="section.title">
              <h3>{{ section.title }}</h3>
              <div class="kana-chart" :class="{ compact: section.title === '요음' }">
                <template v-for="(row, rowIndex) in section.rows" :key="`${section.title}-${rowIndex}`">
                  <div
                    class="kana-cell"
                    v-for="(item, itemIndex) in row"
                    :key="`${section.title}-${rowIndex}-${itemIndex}`"
                    :class="{ empty: !item }"
                  >
                    <template v-if="item">
                      <strong>{{ item.kana }}</strong>
                      <span>{{ item.romaji }}</span>
                      <em>{{ item.readingKo }}</em>
                    </template>
                  </div>
                </template>
              </div>
            </section>
          </template>

          <template v-else>
            <section class="chart-section" v-for="section in combinedChartSections" :key="section.title">
              <h3>{{ section.title }}</h3>
              <div class="kana-chart" :class="{ compact: section.title === '요음' }">
                <template v-for="(row, rowIndex) in section.rows" :key="`${section.title}-${rowIndex}`">
                  <div
                    class="kana-cell"
                    v-for="(item, itemIndex) in row"
                    :key="`${section.title}-${rowIndex}-${itemIndex}`"
                    :class="{ empty: !item }"
                  >
                    <template v-if="item">
                      <strong class="kana-pair"><span>{{ item.kana }}</span><span aria-hidden="true"> / </span><span>{{ item.katakana }}</span></strong>
                      <span>{{ item.romaji }}</span>
                      <em>{{ item.readingKo }}</em>
                    </template>
                  </div>
                </template>
              </div>
            </section>
          </template>
        </div>
      </section>
    </div>

    <div class="chart-overlay" v-if="isVocabularyVisible" @click.self="isVocabularyVisible = false">
      <section class="vocabulary-dialog" aria-label="일본어 단어장">
        <header class="chart-header">
          <div>
            <p class="eyebrow">Vocabulary</p>
            <h2>일본어 단어장</h2>
          </div>
          <button class="chart-close-button" type="button" aria-label="닫기" @click="isVocabularyVisible = false">
            닫기
          </button>
        </header>

        <div class="vocabulary-toolbar">
          <input v-model="vocabularyQuery" type="search" placeholder="단어, 뜻, 발음, 로마자로 검색" aria-label="단어장 검색" />
          <div class="vocabulary-column-controls" aria-label="단어장 표시 항목">
            <button type="button" :class="{ active: vocabularyVisibleColumns.hiragana }" :aria-pressed="vocabularyVisibleColumns.hiragana" @click="toggleVocabularyColumn('hiragana')">히라가나</button>
            <button type="button" :class="{ active: vocabularyVisibleColumns.katakana }" :aria-pressed="vocabularyVisibleColumns.katakana" @click="toggleVocabularyColumn('katakana')">가타카나</button>
            <button type="button" :class="{ active: vocabularyVisibleColumns.reading }" :aria-pressed="vocabularyVisibleColumns.reading" @click="toggleVocabularyColumn('reading')">발음</button>
            <button type="button" :class="{ active: vocabularyVisibleColumns.meaning }" :aria-pressed="vocabularyVisibleColumns.meaning" @click="toggleVocabularyColumn('meaning')">뜻</button>
          </div>
          <strong>{{ vocabularyCards.length }} / {{ wordCards.length }}개</strong>
        </div>

        <div class="vocabulary-table-wrap">
          <table class="vocabulary-table" :class="{ compact: visibleVocabularyColumnCount < 4 }">
            <thead>
              <tr>
                <th v-if="vocabularyVisibleColumns.hiragana" scope="col"><button type="button" @click="toggleVocabularyColumn('hiragana')">히라가나</button></th>
                <th v-if="vocabularyVisibleColumns.katakana" scope="col"><button type="button" @click="toggleVocabularyColumn('katakana')">가타카나</button></th>
                <th v-if="vocabularyVisibleColumns.reading" scope="col"><button type="button" @click="toggleVocabularyColumn('reading')">한글 발음</button></th>
                <th v-if="vocabularyVisibleColumns.meaning" scope="col"><button type="button" @click="toggleVocabularyColumn('meaning')">뜻</button></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="card in vocabularyCards" :key="card.word">
                <td v-if="vocabularyVisibleColumns.hiragana" class="vocabulary-kana">{{ card.word }}</td>
                <td v-if="vocabularyVisibleColumns.katakana" class="vocabulary-kana">{{ toKatakanaText(card.word) }}</td>
                <td v-if="vocabularyVisibleColumns.reading">{{ card.readingKo }}</td>
                <td v-if="vocabularyVisibleColumns.meaning">{{ card.meaningKo }}</td>
              </tr>
              <tr v-if="vocabularyCards.length === 0">
                <td class="vocabulary-empty" :colspan="visibleVocabularyColumnCount">검색 결과가 없습니다.</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
:global(*) {
  box-sizing: border-box;
}

:global(body) {
  margin: 0;
  min-width: 320px;
  color: #1d2733;
  background: #eef2f5;
  font-family:
    Inter, Pretendard, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

button {
  border: 0;
  border-radius: 8px;
  color: #fff;
  background: #28666e;
  font: inherit;
  font-weight: 700;
  cursor: pointer;
  transition:
    transform 0.15s ease,
    background 0.15s ease;
}

button:hover {
  background: #1f535a;
  transform: translateY(-1px);
}

button:active {
  transform: translateY(0);
}

.app-shell {
  display: grid;
  min-height: 100vh;
  place-items: center;
  padding: 24px;
}

.trainer {
  width: min(100%, 520px);
}

.header {
  margin-bottom: 18px;
  text-align: center;
}

.top-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 6px;
}

.top-actions {
  display: flex;
  gap: 8px;
}

.eyebrow {
  margin: 0;
  color: #5d6875;
  font-size: 0.82rem;
  font-weight: 800;
  letter-spacing: 0;
  text-transform: uppercase;
}

.chart-open-button,
.vocabulary-open-button,
.chart-close-button {
  min-height: 36px;
  padding: 0 12px;
  color: #1d2733;
  background: #dce5e9;
  font-size: 0.9rem;
}

.chart-open-button:hover,
.vocabulary-open-button:hover,
.chart-close-button:hover {
  background: #cbd8df;
}

h1 {
  margin: 0;
  font-size: 2rem;
  letter-spacing: 0;
}

.title-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
}

.script-tabs {
  display: inline-grid;
  grid-template-columns: 1fr 1fr;
  gap: 6px;
  padding: 4px;
  border-radius: 8px;
  background: #dce5e9;
}

.script-tabs button {
  min-height: 34px;
  padding: 0 10px;
  color: #1d2733;
  background: transparent;
  font-size: 0.88rem;
}

.script-tabs button:hover {
  background: #cbd8df;
}

.script-tabs button.active {
  color: #fff;
  background: #28666e;
}

.view-tabs,
.level-tabs,
.metric-tabs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
}

.view-tabs,
.metric-tabs {
  margin-bottom: 10px;
}

.level-tabs {
  margin-bottom: 10px;
}

.view-tabs button,
.level-tabs button,
.metric-tabs button {
  min-height: 44px;
  color: #1d2733;
  background: #dce5e9;
}

.metric-tabs button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.metric-tabs span {
  padding: 3px 6px;
  border-radius: 999px;
  color: #28666e;
  background: #fff;
  font-size: 0.72rem;
}

.view-tabs button.active,
.level-tabs button.active,
.metric-tabs button.active {
  color: #fff;
  background: #28666e;
}

.level-label {
  margin: 0 0 14px;
  color: #586572;
  font-weight: 700;
  text-align: center;
}

.card {
  display: grid;
  min-height: 330px;
  align-content: center;
  justify-items: center;
  padding: 28px;
  border: 1px solid #d7dee4;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 16px 38px rgb(29 39 51 / 12%);
  text-align: center;
}

.card.correct {
  border-color: #2f8f46;
  background: #f3fbf5;
  box-shadow: 0 16px 38px rgb(47 143 70 / 18%);
}

.card.wrong {
  border-color: #c24132;
  background: #fff6f4;
  box-shadow: 0 16px 38px rgb(194 65 50 / 18%);
}

.review-badge {
  margin: 0 0 12px;
  padding: 6px 10px;
  border-radius: 999px;
  color: #28666e;
  background: #e4f2f1;
  font-size: 0.82rem;
  font-weight: 800;
}

.ready-title {
  margin: 0;
  font-size: 2rem;
  font-weight: 900;
}

.ready-copy {
  margin: 12px 0 0;
  color: #64717f;
  font-weight: 800;
}

.prompt {
  width: 100%;
  overflow-wrap: anywhere;
  font-size: clamp(4rem, 18vw, 8.5rem);
  font-weight: 800;
  line-height: 1.05;
}

.answer {
  display: grid;
  gap: 8px;
  width: 100%;
  margin-top: 24px;
  font-size: 1.25rem;
}

.answer p {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin: 0;
}

.answer span {
  color: #64717f;
  font-size: 0.95rem;
  font-weight: 800;
}

.message {
  min-height: 24px;
  margin: 18px 0 0;
  padding: 8px 12px;
  border-radius: 8px;
  color: #9b4f21;
  font-weight: 800;
}

.message.correct {
  color: #1f6f35;
  background: #dff4e4;
}

.message.wrong {
  color: #a83225;
  background: #fde1dc;
}

.answer-form {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 96px;
  gap: 8px;
  margin-top: 14px;
}

.answer-form input {
  min-height: 48px;
  width: 100%;
  padding: 0 14px;
  border: 1px solid #cfd8df;
  border-radius: 8px;
  color: #1d2733;
  background: #fff;
  font: inherit;
  font-weight: 800;
}

.answer-form input:focus {
  border-color: #28666e;
  outline: 3px solid rgb(40 102 110 / 18%);
}

.answer-form button {
  min-height: 48px;
}

.actions {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 8px;
  margin-top: 14px;
}

.actions button {
  min-height: 46px;
  padding: 0 12px;
}

.actions .primary-action {
  grid-column: 1 / -1;
  justify-self: center;
  width: min(100%, 220px);
}

.actions .muted-action {
  background: #73808c;
}

.actions .muted-action:hover {
  background: #606c77;
}

.actions .wide {
  grid-column: 1 / -1;
  background: #5f6975;
}

.actions .wide:hover {
  background: #4d5660;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  margin: 14px 0 0;
}

.stats div {
  padding: 14px 8px;
  border: 1px solid #d7dee4;
  border-radius: 8px;
  background: #fff;
  text-align: center;
}

.stats dt {
  color: #64717f;
  font-size: 0.78rem;
  font-weight: 800;
}

.stats dd {
  margin: 6px 0 0;
  font-size: 1.45rem;
  font-weight: 900;
}

.record-panel {
  display: grid;
  gap: 14px;
}

.record-stats {
  margin-top: 0;
}

.record-card {
  padding: 18px;
  border: 1px solid #d7dee4;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 16px 38px rgb(29 39 51 / 12%);
}

.record-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 12px;
}

.record-header .level-label {
  margin: 0 0 4px;
  text-align: left;
}

.record-header h2 {
  margin: 0;
  font-size: 1.35rem;
  letter-spacing: 0;
}

.empty-record {
  margin: 28px 0;
  color: #64717f;
  font-weight: 800;
  text-align: center;
}

.record-list {
  display: grid;
  gap: 8px;
  max-height: 420px;
  overflow: auto;
}

.record-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto auto;
  align-items: center;
  gap: 12px;
  padding: 12px;
  border: 1px solid #e0e6eb;
  border-radius: 8px;
  background: #f8fafb;
}

.record-name {
  display: grid;
  gap: 2px;
  min-width: 0;
}

.record-name strong {
  font-size: 1.35rem;
}

.record-name span,
.record-counts {
  color: #64717f;
  font-size: 0.88rem;
  font-weight: 800;
}

.record-counts {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
  gap: 6px 10px;
}

.record-rate {
  color: #28666e;
  font-size: 1.2rem;
}

.chart-overlay {
  position: fixed;
  inset: 0;
  z-index: 20;
  display: grid;
  place-items: center;
  padding: 18px;
  background: rgb(29 39 51 / 54%);
}

.chart-dialog {
  display: grid;
  grid-template-rows: auto auto auto minmax(0, 1fr);
  width: min(100%, 920px);
  max-height: min(92vh, 860px);
  overflow: hidden;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 24px 60px rgb(29 39 51 / 28%);
}

.vocabulary-dialog {
  display: grid;
  grid-template-rows: auto auto minmax(0, 1fr);
  width: min(100%, 1020px);
  height: min(92vh, 860px);
  max-height: min(92vh, 860px);
  overflow: hidden;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 24px 60px rgb(29 39 51 / 28%);
}

.chart-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 18px;
  border-bottom: 1px solid #e0e6eb;
}

.chart-header h2 {
  margin: 4px 0 0;
  font-size: 1.45rem;
}

.chart-controls {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;
  padding: 12px 18px;
  border-bottom: 1px solid #e0e6eb;
  background: #f8fafb;
}

.chart-mode-tabs {
  display: flex;
  gap: 8px;
  padding: 12px 18px;
  border-bottom: 1px solid #e0e6eb;
}

.chart-mode-tabs button {
  min-height: 36px;
  padding: 0 14px;
  color: #52606d;
  background: #e8eef1;
}

.chart-mode-tabs button.active {
  color: #fff;
  background: #28666e;
}

.chart-controls button {
  min-height: 36px;
  padding: 0 12px;
  background: #28666e;
}

.chart-controls span {
  min-width: 56px;
  color: #28666e;
  font-weight: 900;
  text-align: center;
}

.chart-scroll {
  overflow: auto;
  padding: 18px;
}

.vocabulary-toolbar {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 18px;
  border-bottom: 1px solid #e0e6eb;
  background: #f8fafb;
}

.vocabulary-toolbar input {
  flex: 1;
  min-width: 0;
  min-height: 40px;
  padding: 0 12px;
  border: 1px solid #c8d3da;
  border-radius: 6px;
  color: #1d2733;
  background: #fff;
  font: inherit;
}

.vocabulary-toolbar input:focus {
  outline: 2px solid #28666e;
  outline-offset: 1px;
}

.vocabulary-toolbar strong {
  min-width: 76px;
  color: #28666e;
  text-align: right;
}

.vocabulary-column-controls {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.vocabulary-column-controls button {
  min-height: 32px;
  padding: 0 8px;
  color: #667381;
  background: #e4ebef;
  font-size: 0.78rem;
}

.vocabulary-column-controls button.active {
  color: #fff;
  background: #28666e;
}

.vocabulary-table-wrap {
  overflow: auto;
}

.vocabulary-table {
  width: 100%;
  min-width: 620px;
  border-collapse: collapse;
  text-align: left;
}

.vocabulary-table.compact {
  min-width: 0;
}

.vocabulary-table th,
.vocabulary-table td {
  padding: 12px 18px;
  border-bottom: 1px solid #e0e6eb;
}

.vocabulary-table th {
  position: sticky;
  top: 0;
  z-index: 1;
  color: #52606d;
  background: #f8fafb;
  font-size: 0.82rem;
}

.vocabulary-table th button {
  padding: 0;
  color: inherit;
  background: transparent;
  font: inherit;
  font-weight: 900;
}

.vocabulary-table th button:hover {
  color: #28666e;
}

.vocabulary-table tbody tr:hover {
  background: #f3f7f8;
}

.vocabulary-kana {
  color: #1d2733;
  font-size: 1.25rem;
  font-weight: 900;
}

.vocabulary-empty {
  padding: 36px !important;
  color: #667381;
  text-align: center;
}

.chart-section + .chart-section {
  margin-top: 24px;
}

.chart-section h3 {
  margin: 0 0 10px;
  color: #1d2733;
  font-size: 1.1rem;
}

.kana-chart {
  display: grid;
  grid-template-columns: repeat(5, calc(104px * var(--chart-zoom)));
  gap: calc(8px * var(--chart-zoom));
  width: max-content;
}

.kana-chart.compact {
  grid-template-columns: repeat(3, calc(124px * var(--chart-zoom)));
}

.kana-cell {
  display: grid;
  justify-items: center;
  align-content: center;
  min-height: calc(96px * var(--chart-zoom));
  padding: calc(10px * var(--chart-zoom));
  border: 1px solid #d9e1e7;
  border-radius: 8px;
  background: #f8fafb;
  text-align: center;
}

.kana-cell.empty {
  border-style: dashed;
  background: #eef2f5;
}

.kana-cell strong {
  color: #1d2733;
  font-size: calc(2.15rem * var(--chart-zoom));
  line-height: 1;
}

.kana-cell .kana-pair {
  display: flex;
  gap: calc(4px * var(--chart-zoom));
  align-items: center;
  white-space: nowrap;
  font-size: calc(1.65rem * var(--chart-zoom));
}

.kana-cell span {
  margin-top: calc(7px * var(--chart-zoom));
  color: #28666e;
  font-size: calc(0.9rem * var(--chart-zoom));
  font-weight: 900;
}

.kana-cell em {
  margin-top: calc(3px * var(--chart-zoom));
  color: #667381;
  font-size: calc(0.84rem * var(--chart-zoom));
  font-style: normal;
  font-weight: 800;
}

@media (max-width: 430px) {
  .app-shell {
    display: block;
    min-height: 100dvh;
    padding: 16px;
  }

  .trainer {
    margin: 0 auto;
  }

  h1 {
    font-size: 1.55rem;
  }

  .top-bar {
    align-items: flex-start;
  }

  .top-actions {
    gap: 6px;
  }

  .title-row {
    align-items: stretch;
    justify-content: center;
  }

  .script-tabs {
    width: min(100%, 220px);
  }

  .chart-overlay {
    align-items: stretch;
    padding: 10px;
  }

  .chart-dialog {
    max-height: calc(100dvh - 20px);
  }

  .vocabulary-dialog {
    height: calc(100dvh - 20px);
    max-height: calc(100dvh - 20px);
  }

  .chart-header {
    padding: 14px;
  }

  .chart-header h2 {
    font-size: 1.2rem;
  }

  .chart-controls {
    padding: 10px 14px;
  }

  .chart-mode-tabs {
    padding: 10px 14px;
  }

  .chart-mode-tabs button {
    flex: 1;
    padding: 0 8px;
    font-size: 0.82rem;
  }

  .chart-scroll {
    padding: 14px;
  }

  .vocabulary-toolbar {
    gap: 8px;
    padding: 10px 14px;
  }

  .vocabulary-column-controls {
    order: 3;
    width: 100%;
  }

  .vocabulary-toolbar strong {
    min-width: 62px;
    font-size: 0.82rem;
  }

  .vocabulary-table th,
  .vocabulary-table td {
    padding: 10px 14px;
  }

  .kana-chart {
    grid-template-columns: repeat(5, calc(82px * var(--chart-zoom)));
  }

  .kana-chart.compact {
    grid-template-columns: repeat(3, calc(96px * var(--chart-zoom)));
  }

  .kana-cell {
    min-height: calc(78px * var(--chart-zoom));
  }

  .card {
    min-height: 230px;
    padding: 20px 16px;
  }

  .prompt {
    font-size: clamp(3.5rem, 24vw, 6.5rem);
  }

  .answer-form {
    grid-template-columns: minmax(0, 1fr) 76px;
  }

  .answer-form input {
    min-height: 44px;
    padding: 0 12px;
  }

  .answer-form button {
    min-height: 44px;
  }

  .stats {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 6px;
  }

  .stats div {
    padding: 10px 4px;
  }

  .stats dt {
    font-size: 0.68rem;
    line-height: 1.25;
  }

  .stats dd {
    font-size: 1.2rem;
  }

  .record-row {
    grid-template-columns: minmax(0, 1fr) auto;
  }

  .record-counts {
    grid-column: 1 / -1;
    grid-row: 2;
    justify-content: flex-start;
  }
}
</style>
