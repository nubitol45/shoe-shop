<template>
    <div class="fixed bottom-0 bg-white w-full">
        <div class="flex py-5 px-10 gap-5 w-full justify-end">
            <div>
                <h3 class="font-bold text-gray-500">Total</h3>
                <b>Rp{{ totalPrice | curency }}</b>
            </div>

            <button
                @click="checkout"
                class="bg-white text-yellow-600 px-5 py-2 font-bold border-yellow-600 border rounded-md"
            >
                Beli Langsung
            </button>
            <form @submit.prevent="addCartItem">
                <loading-button
                    :loading="sending"
                    type="submit"
                    class="bg-yellow-600 text-white px-5 py-2 font-bold border rounded-md"
                >
                    + Keranjang
                </loading-button>
            </form>
        </div>
    </div>
</template>

<script>
import LoadingButton from '@/components/LoadingButton';
export default {
    components: {
        LoadingButton,
    },
    props: ['totalPrice', 'item'],
    data() {
        return {
            sending: false,
            form: [],
        };
    },
    methods: {
        addCartItem() {
            this.$inertia.post(
                this.route('cart.store'),
                { itemId: this.item.item.id, quantity: this.item.quantity },
                {
                    onStart: () => (this.sending = true),
                    onFinish: () => (this.sending = false),
                }
            );
        },
        checkout() {
            let form = [];
            form.push({
                id: this.item.item.id,
                quantity: this.item.quantity,
            });
            this.$inertia.post(this.route('buy_directly'), {
                item: form,
                lots: false,
            });
        },
    },
};
</script>

<style></style>
