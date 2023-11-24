<script setup lang="ts">
import { ref, computed, watch } from 'vue'

interface Word {
    name: string
    length: number
    sort: string
    unvoiced: boolean
}

const newWordName = ref('')
const dakuon = "がぎぐげござじずぜぞだぢづでどばびぶべぼぱぴぷぺぽ"

const nameToWord = (str: string) => {
    const w = str
    const sorted = w.split('').sort().join('')
    const len = w.length
    const isUnvoiced = w.split('').some(char => dakuon.includes(char))
    const newWord = {name: w, length: len, sort: sorted, unvoiced: isUnvoiced}
    return newWord
}

const selectedDictionary = ref("basic");
const searchResult = ref([]);

const inputRegularExpression = ref('')
const searchWordsWithRegEx = () => {
    const url = `http://localhost:8080/search/${encodeURIComponent(inputRegularExpression.value)}?dict=${selectedDictionary.value}`;

    fetch(url)
        .then((response) => response.json())
        .then((data) => {
            searchResult.value = data;
        })
        .catch((error) => console.error("Error:", error));
};

</script>

<template>
    <div>
        辞書選択
        <div>
            <input type="radio" id="basic" name="dictionary" value="basic" v-model="selectedDictionary" checked />
            <label for="basic">一般語</label>
        </div>
        <div>
            <input type="radio" id="test" name="dictionary" value="test" v-model="selectedDictionary" />
            <label for="test">テスト</label>
        </div>
    </div>
    <div>
        <label>
            正規表現で検索
            <input v-model="inputRegularExpression" type="text" />
        </label>
        <button @click="searchWordsWithRegEx">検索</button>
    </div>
    <div>検索結果</div>
    <ul>
        <li v-for="word in searchResult" :key="word">
            <div>{{ word }}</div>
        </li>
    </ul>
</template>

<style></style>