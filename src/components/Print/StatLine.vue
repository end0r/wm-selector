<template>
  <tr :class="{ 'upgrade': (parent !== undefined) }">
    <td class="points-cost" v-if="used">{{pointsCost()}}</td>
    <td class="number" v-if="used">{{troop.number}}</td>
    <td class="troop">{{name}}</td>
    <td class="type">{{troop.type}}</td>
    <td class="attack">{{troop.attack || '-' }}</td>
    <td class="range">{{troop.range || '-'}}</td>
    <td class="hits">{{troop.hits || '-'}}</td>
    <td class="armour">{{troop.armour || '-'}}</td>
    <td class="command">{{troop.command || '-'}}</td>
    <td class="size">{{troop.size || '-' }}</td>
    <td class="points">{{points}}</td>
    <td class="minMax">{{troop.minMax}}</td>
    <td class="special">{{special(name, troop.specialRules)}}</td>
  </tr>
</template>

<script>
import store from '@/store';

export default {
  name: 'StatLines',
  computed: {
    points () {
      let points;

      if (this.troop.pointsValue !== undefined) {
        points = this.troop.points[store.getters.units[this.parent][this.troop.pointsValue] || '-'];
      } else {
        points = this.troop.points;
      }

      return points;
    }
  },
  methods: {
    pointsCost () {
      // check if this is an upgrade who's price is included in a preceding line
      return this.parent !== undefined ? '(' + this.troop.pointsCost + ')' : this.troop.pointsCost;
    },
    special: (name, specialRules) => {
      return [name].concat(specialRules).reduce((special, name) => {
        if (store.getters.specialRules[name]) {
          special.push(store.getters.specialRules[name].order);
        }

        return special;
      }, []).sort((a, b) => a - b).map((order) => '*' + order).join(', ') || '-';
    }
  },
  props: ['name', 'troop', 'used', 'parent']
};
</script>

<style lang="scss">
</style>
