<template>
	<div class="maskcore">
		<div class="main">
			<div class="fileImporter">
				<input type="file" ref="file" @change="previewImage()" />
			</div>
			<div class="organizer">
				<img v-if="this.url" :src="this.url" class="imageUser" />
			</div>
			<div>
				<p v-if="this.nulo">
					Favor escolher um arquivo antes de enviar
				</p>
			</div>
			<b-button class="botao" @click="maskDetect()" variant="info">
				Verificar Máscara
			</b-button>
			<p v-if="this.maior" class="status">Status: {{ this.maior.descricao }}</p>
		</div>
	</div>
</template>

<script>
	export default {
		name: "MaskDetector",
		data() {
			return {
				file: null,
				response: null,
				url: null,
				nulo: false,
				maior: null,
			};
		},
		methods: {
			previewImage() {
                if(this.$refs.file.files[0] == null) {
                    return;
                }
				this.file = this.$refs.file.files[0];
				this.url = URL.createObjectURL(this.file);
				this.nulo = false;
			},
			async maskDetect() {
				if (this.file == null) {
					this.nulo = true;
					return;
				}
                let i = 0;
				const formData = new FormData();
				formData.append("image", this.file);
				await this.$axios
					.$post("http://127.0.0.1:5000/", formData, {
						headers: {
							"Content-Type": "multipart/form-data",
						},
					})
					.then((response) => {
						console.log("pinto", response);
						console.log(response.resultados);
						this.response = response.resultados;
						this.maior = this.response[0];
						for (i=0; i<this.response.length; i++) {
                            console.log(this.maior.valor)
                            console.log(this.response[i])
							if (this.response[i].valor > this.maior.valor) {
								this.maior = this.response[i];
							}
						}
					})
					.catch((e) => {
						console.log("caiu aqui: ", e);
					});
			},
		},
	};
</script>

<style>
	.main {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		margin-top: 50px;
	}
	.fileImporter {
		background-color: rgb(5, 92, 255);
		padding: 10px;
		border-color: transparent;
		border-radius: 10px;
	}
	.maskcore {
		color: white;
	}
	.imageUser {
		height: 250px;
		border-color: transparent;
		border-radius: 10px;
	}
	.organizer {
		margin-top: 20px;
		display: grid;
		text-align: center;
	}
	.botao {
		margin-top: 20px;
	}
    .status {
        margin-top: 20px
    }
</style>
