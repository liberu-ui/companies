<template>
    <card collapsible
        :collapsed="collapsed">
        <card-header class="has-background-light">
            <template #title>
                <span class="icon is-small mr-1">
                    <fa icon="address-card"/>
                </span>
                {{ displayTitle }}
            </template>
            <template #controls>
                <card-refresh @refresh="fetch"/>
                <card-badge :label="count"/>
                <card-collapse/>
            </template>
        </card-header>
        <card-content>
            <people :id="id"
                :query="query"
                @update="count = $refs.people.count; $refs.card.resize()"
                ref="people"/>
        </card-content>
    </card>
</template>

<script>
import { mapState } from 'vuex';
import { FontAwesomeIcon as Fa } from '@fortawesome/vue-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faAddressCard, faPlusSquare } from '@fortawesome/free-solid-svg-icons';
import {
    Card, CardHeader, CardRefresh, CardCollapse, CardBadge, CardContent,
} from '@liberu-ui/card/bulma';
import People from './People.vue';

library.add(faAddressCard, faPlusSquare);

export default {
    name: 'PeopleCard',

    components: {
        Fa,
        Card,
        CardHeader,
        CardRefresh,
        CardCollapse,
        CardBadge,
        CardContent,
        People,
    },

    inject: ['i18n'],

    props: {
        id: {
            type: Number,
            required: true,
        },
        open: {
            type: Boolean,
            default: false,
        },
        title: {
            type: String,
            default: null,
        },
    },

    data: () => ({
        query: '',
        count: 0,
    }),

    computed: {
        ...mapState('layout', ['isMobile']),
        displayTitle() {
            return !this.isMobile
                ? this.title || this.i18n('People')
                : null;
        },
        isEmpty() {
            return this.count === 0;
        },
    },

    watch: {
        count() {
            this.$refs.card.resize();
        },
    },
};
</script>
