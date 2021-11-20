<template>
	<div class="mamaaqui">
        <h1>MaskDetector</h1>
        <h4>Descubra se você sabe usar uma máscara de maneira apropriada!</h4>
		<MaskDetector />
	</div>
</template>

<script>
    import MaskDetector from '../components/MaskDetector.vue';
	export default {
        components: {
            MaskDetector,
        }

    };
</script>

<style scoped>
.mamaaqui {
    background-color:rgb(16, 16, 34);
    height: 100vh;
    width: 100vw;
    color: white;
    display: block;
    text-align: center;
}
</style>
