<script setup>
import { computed, ref, watch } from 'vue'

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
]

const level = ref(1)
const currentCard = ref(null)
const isAnswerVisible = ref(false)
const isAnswered = ref(false)
const reviewMode = ref(false)
const message = ref('')
const todayCount = ref(0)
const correctCount = ref(0)
const wrongCount = ref(0)
const streakCount = ref(0)
const wrongCards = ref([])

const activeCards = computed(() => {
  if (reviewMode.value) {
    return wrongCards.value
  }

  return level.value === 1 ? kanaCards : wordCards
})

const cardText = computed(() => {
  if (!currentCard.value) return ''
  return currentCard.value.kana || currentCard.value.word
})

const levelLabel = computed(() => (level.value === 1 ? '히라가나 음 카드' : '일본어 단어 카드'))

function getCardKey(card) {
  return card.kana || card.word
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

function showAnswer() {
  isAnswerVisible.value = true
  message.value = ''
}

function nextCard() {
  const cards = activeCards.value

  if (reviewMode.value && cards.length === 0) {
    message.value = '틀린 카드가 없습니다'
    reviewMode.value = false
    currentCard.value = pickRandomCard(activeCards.value)
    return
  }

  currentCard.value = pickRandomCard(cards)
  isAnswerVisible.value = false
  isAnswered.value = false
  message.value = reviewMode.value ? '틀린 카드 복습 중입니다' : ''
}

function markCorrect() {
  if (isAnswered.value) return

  todayCount.value += 1
  correctCount.value += 1
  streakCount.value += 1
  isAnswered.value = true
  message.value = '맞음으로 기록했습니다'
}

function markWrong() {
  if (isAnswered.value) return

  todayCount.value += 1
  wrongCount.value += 1
  streakCount.value = 0
  isAnswered.value = true

  if (!wrongCards.value.some((card) => getCardKey(card) === getCardKey(currentCard.value))) {
    wrongCards.value.push(currentCard.value)
  }

  message.value = '틀린 카드에 저장했습니다'
}

function startWrongReview() {
  if (wrongCards.value.length === 0) {
    message.value = '틀린 카드가 없습니다'
    return
  }

  reviewMode.value = true
  nextCard()
}

function changeLevel(nextLevel) {
  level.value = nextLevel
}

watch(level, () => {
  reviewMode.value = false
  nextCard()
})

nextCard()
</script>

<template>
  <main class="app-shell">
    <section class="trainer">
      <header class="header">
        <p class="eyebrow">Hiragana Speed Trainer</p>
        <h1>히라가나 반응속도 훈련</h1>
      </header>

      <div class="level-tabs" aria-label="난이도 선택">
        <button :class="{ active: level === 1 }" type="button" @click="changeLevel(1)">
          난이도 1
        </button>
        <button :class="{ active: level === 2 }" type="button" @click="changeLevel(2)">
          난이도 2
        </button>
      </div>

      <p class="level-label">{{ levelLabel }}</p>

      <article class="card">
        <p class="review-badge" v-if="reviewMode">틀린 카드 복습</p>
        <div class="prompt">{{ cardText }}</div>

        <div class="answer" v-if="isAnswerVisible && currentCard">
          <p><span>발음</span>{{ currentCard.readingKo }}</p>
          <p><span>로마자</span>{{ currentCard.romaji }}</p>
          <p v-if="currentCard.meaningKo"><span>뜻</span>{{ currentCard.meaningKo }}</p>
        </div>

        <p class="message" v-if="message">{{ message }}</p>
      </article>

      <div class="actions">
        <button v-if="!isAnswerVisible" type="button" @click="showAnswer">확인</button>
        <template v-else-if="!isAnswered">
          <button type="button" @click="markCorrect">맞음</button>
          <button type="button" @click="markWrong">틀림</button>
        </template>
        <button v-else type="button" @click="nextCard">다음</button>
        <button class="wide" type="button" @click="startWrongReview">틀린 카드 복습</button>
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
        <div>
          <dt>연속 정답</dt>
          <dd>{{ streakCount }}</dd>
        </div>
      </dl>
    </section>
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

.eyebrow {
  margin: 0 0 6px;
  color: #5d6875;
  font-size: 0.82rem;
  font-weight: 800;
  letter-spacing: 0;
  text-transform: uppercase;
}

h1 {
  margin: 0;
  font-size: 2rem;
  letter-spacing: 0;
}

.level-tabs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-bottom: 10px;
}

.level-tabs button {
  min-height: 44px;
  color: #1d2733;
  background: #dce5e9;
}

.level-tabs button.active {
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

.review-badge {
  margin: 0 0 12px;
  padding: 6px 10px;
  border-radius: 999px;
  color: #28666e;
  background: #e4f2f1;
  font-size: 0.82rem;
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
  color: #9b4f21;
  font-weight: 800;
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

.actions .wide {
  grid-column: 1 / -1;
  background: #5f6975;
}

.actions .wide:hover {
  background: #4d5660;
}

.stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
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

@media (max-width: 430px) {
  .app-shell {
    padding: 16px;
  }

  h1 {
    font-size: 1.55rem;
  }

  .card {
    min-height: 300px;
    padding: 22px 18px;
  }

  .stats {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
