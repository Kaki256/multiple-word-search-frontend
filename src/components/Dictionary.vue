<script setup lang="ts">
import { ref, computed } from 'vue'

interface Word {
    name: string
    length: number
    sort: string
    unvoiced: boolean
}

const words = ref<Word[]>([
    { name: "りんご", length: 3, sort: "ごりん", unvoiced: true},
    { name: "ごりら", length: 3, sort: "ごらり", unvoiced: true},
    { name: "きょうと", length: 4, sort: "うきとょ", unvoiced: false},
    { name: "ひだりうちわ", length: 6, sort: "うだちひりわ", unvoiced: false },
    { name: "ひざら", length: 3, sort: "ざひら", unvoiced: false },
    { name: "すいす", length: 3, sort: "いすす", unvoiced: true },
    { name: "ちゅうう", length: 4, sort: "ううちゅ", unvoiced: true },
    { name: "ぎんせん", length: 4, sort: "ぎせんん", unvoiced: false },
    { name: "とうだい", length: 4, sort: "いうだと", unvoiced: false },
    { name: "はんれい", length: 4, sort: "いはれん", unvoiced: true },
    { name: "こころくばり", length: 6, sort: "くここばりろ", unvoiced: false },
    { name: "けんさつ", length: 4, sort: "けさつん", unvoiced: true },
    { name: "ゆうてぃりてぃい", length: 8, sort: "ぃぃいうててゆり", unvoiced: true },
    { name: "くろすおーばー", length: 7, sort: "おくすばろーー", unvoiced: false },
    { name: "こうせつ", length: 4, sort: "うこせつ", unvoiced: true },
    { name: "まいまい", length: 4, sort: "いいまま", unvoiced: true },
    { name: "ちさん", length: 3, sort: "さちん", unvoiced: true },
    { name: "とろう", length: 3, sort: "うとろ", unvoiced: true },
    { name: "しそう", length: 3, sort: "うしそ", unvoiced: true },
    { name: "ざんそ", length: 3, sort: "ざそん", unvoiced: false },
    { name: "はんかち", length: 4, sort: "かちはん", unvoiced: true },
    { name: "たすけ", length: 3, sort: "けすた", unvoiced: true },
    { name: "むじょう", length: 4, sort: "うじむょ", unvoiced: false },
    { name: "すみび", length: 3, sort: "すびみ", unvoiced: false },
    { name: "ひこうき", length: 4, sort: "うきこひ", unvoiced: true },
    { name: "しゃかく", length: 4, sort: "かくしゃ", unvoiced: true },
    { name: "こより", length: 3, sort: "こより", unvoiced: true },
    { name: "ないじょ", length: 4, sort: "いじなょ", unvoiced: false },
    { name: "どくざ", length: 3, sort: "くざど", unvoiced: false },
    { name: "うぶすな", length: 4, sort: "うすなぶ", unvoiced: false },
    { name: "うたいて", length: 4, sort: "いうたて", unvoiced: true },
    { name: "じゅうざ", length: 4, sort: "うざじゅ", unvoiced: false },
    { name: "これくと", length: 4, sort: "くことれ", unvoiced: true },
    { name: "きんりょう", length: 5, sort: "うきょりん", unvoiced: true },
    { name: "のぼせ", length: 3, sort: "せのぼ", unvoiced: false },
    { name: "かわじゃり", length: 5, sort: "かじゃりわ", unvoiced: false },
    { name: "めいかい", length: 4, sort: "いいかめ", unvoiced: true },
    { name: "いもばたけ", length: 5, sort: "いけたばも", unvoiced: false }
])

const resultOfSearch = ref<Word[]>([

])

const newWordName = ref('')
const inputRegularExpression = ref('')
const dakuon = "がぎぐげござじずぜぞだぢづでどばびぶべぼぱぴぷぺぽ"

const nameToWord = (str: string) => {
    const w = str
    const sorted = w.split('').sort().join('')
    const len = w.length
    const isUnvoiced = w.split('').some(char => dakuon.includes(char))
    const newWord = {name: w, length: len, sort: sorted, unvoiced: isUnvoiced}
    return newWord
}

const addWord = () => {
    if (newWordName.value == '') {return }

    words.value.push(nameToWord(newWordName.value))
    newWordName.value = ''
}

const wordSearch = () => {
    if (inputRegularExpression.value == '') {return }
    const regEx = new RegExp(inputRegularExpression.value)
    // resultOfSearch.value = words.value.filter(str => regEx.test(str.name))
}

</script>

<template>
    <div>
        辞書選択
        <div>
            <input type="radio" id="basic" name="dictionary" value="basic" checked />
            <label for="basic">一般語</label>
        </div>
        <div>
            <input type="radio" id="test" name="dictionary" value="test" />
            <label for="test">テスト</label>
        </div>
    </div>
    <div>
        <label>
            追加したい単語
            <input v-model="newWordName" type="text" />
        </label>
        <button @click="addWord">追加</button>
        (※サーバー側では変更されません)
    </div>
    <div>
        <label>
            正規表現で検索
            <input v-model="inputRegularExpression" type="text" />
        </label>
        <button @click="wordSearch">検索</button>
    </div>
    <div>検索結果</div>
    <ul>
        <li v-for="word in resultOfSearch" :key="word.name">
            <div>{{ word.name }}</div>
        </li>
    </ul>
    <div>Dictionary</div>
    <ul>
        <li v-for="word in words" :key="word.name">
            <div>{{ word.name }}</div>
        </li>
    </ul>
</template>

<style></style>