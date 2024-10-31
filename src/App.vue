<script setup lang="ts">
import { ref, computed } from 'vue';
import rawPdf from './assets/pdf/character_cutter.pdf';
import { degrees, PDFDocument, rgb, StandardFonts } from 'pdf-lib';

const pdfElements = ref([]);

// getPdf();
async function getPdf() {
    const pdfDoc = await PDFDocument.load(
        await fetch(rawPdf).then((res) => res.arrayBuffer())
    );

    // The PDF will always have exactly one page
    const page = pdfDoc.getPages()[0];

    page.drawText('Creating PDFs in JavaScript is awesome!', {
        x: 50,
        y: height - 4 * fontSize,
        size: 30,
        font: 'Arial',
        color: rgb(0, 0.53, 0.71),
    });
}


test();
async function test() {
    const url = 'https://pdf-lib.js.org/assets/with_update_sections.pdf';
    const existingPdfBytes = await fetch(url).then((res) => res.arrayBuffer());

    const pdfDoc = await PDFDocument.load(existingPdfBytes);
    const helveticaFont = await pdfDoc.embedFont(StandardFonts.Helvetica);

    const pages = pdfDoc.getPages();
    const firstPage = pages[0];
    const { width, height } = firstPage.getSize();
    firstPage.drawText('This text was added with JavaScript!', {
        x: 5,
        y: height / 2 + 300,
        size: 50,
        font: helveticaFont,
        color: rgb(0.95, 0.1, 0.1),
        rotate: degrees(-45),
    });

    const pdfBytes = await pdfDoc.save();
}
</script>

<template>
    <div>
        <pre>{{ pdfElements }}</pre>
    </div>
</template>

<style scoped lang="scss">
@import './assets/scss/reset.scss';
@import './assets/scss/styles.scss';
</style>
