<template>
    <div class="plugin-editions-edition">
        <div class="description">
            <h4 class="edition-name">{{edition.name}}</h4>
            <div class="price">
                <template v-if="!isPluginEditionFree(edition)">
                    {{edition.price|currency}}
                </template>
                <template v-else>
                    {{ "Free"|t('app') }}
                </template>
            </div>

            <p v-if="!isPluginEditionFree(edition)" class="-mt-8 py-6 text-grey-dark">
                {{ "Price includes 1 year of updates."|t('app') }}<br />
                {{ "{renewalPrice}/year per site for updates after that."|t('app', {renewalPrice: $options.filters.currency(edition.renewalPrice)}) }}
            </p>

            <ul v-if="edition.features.length > 0">
                <li v-for="(feature, key) in edition.features" :key="key">
                    <font-awesome-icon icon="check"></font-awesome-icon>
                    {{feature.name}}

                    <info-hud>
                        {{feature.description}}
                    </info-hud>
                </li>
            </ul>
        </div>

        <plugin-actions :plugin="plugin" :edition="edition"></plugin-actions>
    </div>
</template>

<script>
    import {mapState, mapGetters} from 'vuex'
    import PluginActions from './PluginActions'
    import InfoHud from './InfoHud'

    export default {

        props: ['plugin', 'edition'],

        components: {
            PluginActions,
            InfoHud,
        },

        computed: {

            ...mapState({
                cart: state => state.cart.cart,
            }),

            ...mapGetters({
                isPluginEditionFree: 'pluginStore/isPluginEditionFree',
            }),

        },

    }
</script>

<style lang="scss">
    .plugin-editions-edition {
        @apply .border .border-grey-light .border-solid .p-8 .rounded .text-center .flex .flex-col;

        .description {
            @apply .flex-1;

            .edition-name {
                @apply .border-b .border-grey-light .border-solid .text-grey-dark .inline-block .py-1 .uppercase .text-lg .font-bold;
            }

            .price {
                @apply .text-3xl .font-bold .my-8;
            }

            ul {
                @apply .text-left .mb-8;

                li {
                    @apply .py-2 .border-b .border-grey-lighter .border-solid;

                    &:first-child {
                        @apply .border-t;
                    }

                    svg[data-icon="info-circle"] {
                        path {
                            fill: #ccc;
                        }
                    }
                }
            }
        }
    }
</style>