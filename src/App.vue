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
const isStarted = ref(false)
const questionStartedAt = ref(null)
const currentView = ref('trainer')
const recordMetric = ref('accuracy')
const recordSortDirection = ref('desc')
const reviewMode = ref(false)
const message = ref('')
const answerInput = ref('')
const answerInputElement = ref(null)
const todayCount = ref(0)
const correctCount = ref(0)
const wrongCount = ref(0)
const wrongCards = ref([])
const cardStats = ref({})

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

const recordCards = computed(() => (level.value === 1 ? kanaCards : wordCards))

const recordSortLabel = computed(() => (recordSortDirection.value === 'desc' ? '내림차순' : '오름차순'))

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

  if (!wrongCards.value.some((wrongCard) => getCardKey(wrongCard) === getCardKey(card))) {
    wrongCards.value.push(card)
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
  focusAnswerInput()
}

function markCorrect(responseMs = getElapsedResponseMs()) {
  if (isAnswered.value || !currentCard.value) return

  todayCount.value += 1
  correctCount.value += 1
  recordCardResult(currentCard.value, 'correct', responseMs)
  questionStartedAt.value = null
  isAnswered.value = true
  message.value = `정답입니다 · ${formatResponseTime(responseMs)}`
}

function startWrongReview() {
  if (wrongCards.value.length === 0) {
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

function changeLevel(nextLevel) {
  level.value = nextLevel
}

watch(level, () => {
  reviewMode.value = false
  isStarted.value = false
  currentCard.value = null
  isAnswerVisible.value = false
  isAnswered.value = false
  answerInput.value = ''
  questionStartedAt.value = null
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
        <p class="eyebrow">Hiragana Speed Trainer</p>
        <h1>히라가나 반응속도 훈련</h1>
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

        <article class="card">
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

          <p class="message" v-if="message">{{ message }}</p>
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
  color: #9b4f21;
  font-weight: 800;
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
