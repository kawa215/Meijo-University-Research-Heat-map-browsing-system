<template>
  <component
    :is="item.component"
    v-if="item.component && !isItemHidden"
    v-bind="item.props"
  />
  <div
    v-else-if="item.header && !isItemHidden"
    class="vsm--header"
    :class="item.class"
    v-bind="item.attributes"
  >
    {{ item.title }}
  </div>
  <div
    v-else-if="!isItemHidden"
    class="vsm--item"
    :class="[{ 'vsm--item_open': show }]"
    v-on="
      disableHover && isCollapsed
        ? { click: mouseEnterEvent }
        : { mouseover: mouseEnterEvent }
    "
    @mouseout="mouseLeaveEvent"
  >
    <template v-if="isRouterLink && !item.disabled">
      <router-link
        :class="itemLinkClass"
        :to="itemLinkHref"
        v-bind="item.attributes"
        @click.native="clickEvent"
      >
        <template v-if="item.icon && !isMobileItem">
          <i
            v-if="typeof item.icon === 'string' || item.icon instanceof String"
            class="vsm--icon"
            :class="item.icon"
          />
          <component
            :is="item.icon.element ? item.icon.element : 'i'"
            v-else
            class="vsm--icon"
            :class="item.icon.class"
            v-bind="item.icon.attributes"
          >
            {{ item.icon.text }}
          </component>
        </template>
        <template
          v-if="(isCollapsed && !isFirstLevel) || !isCollapsed || isMobileItem"
        >
          <component
            :is="item.badge.element ? item.badge.element : 'span'"
            v-if="item.badge"
            :style="[
              rtl
                ? item.child
                  ? { 'margin-left': '30px' }
                  : ''
                : item.child
                ? { 'margin-right': '30px' }
                : ''
            ]"
            class="vsm--badge"
            :class="item.badge.class"
            v-bind="item.badge.attributes"
          >
            {{ item.badge.text }}
          </component>
          <span class="vsm--title">{{ item.title }}{{ item.title }}</span>
          <div
            v-if="item.child"
            class="vsm--arrow"
            :class="[
              { 'vsm--arrow_open': show },
              { 'vsm--arrow_slot': $slots['dropdown-icon'] }
            ]"
          >
            <slot name="dropdown-icon" />
          </div>
        </template>
      </router-link>
    </template>
    <template v-else>
      <component
        :is="itemLinkHref && !item.disabled ? 'a' : 'span'"
        :class="itemLinkClass"
        :href="itemLinkHref && !item.disabled ? itemLinkHref : undefined"
        :tabindex="item.disabled ? -1 : 0"
        role="link"
        v-bind="item.attributes"
        @click="clickEvent"
        @keydown.enter="clickEvent"
      >
        <template v-if="item.icon && !isMobileItem">
          <i
            v-if="typeof item.icon === 'string' || item.icon instanceof String"
            class="vsm--icon"
            :class="item.icon"
          />
          <component
            :is="item.icon.element ? item.icon.element : 'i'"
            v-else
            class="vsm--icon"
            :class="item.icon.class"
            v-bind="item.icon.attributes"
          >
            {{ item.icon.text }}
          </component>
        </template>
        <template
          v-if="(isCollapsed && !isFirstLevel) || !isCollapsed || isMobileItem"
        >
          <component
            :is="item.badge.element ? item.badge.element : 'span'"
            v-if="item.badge"
            :style="[
              rtl
                ? item.child
                  ? { 'margin-left': '30px' }
                  : ''
                : item.child
                ? { 'margin-right': '30px' }
                : ''
            ]"
            class="vsm--badge"
            :class="item.badge.class"
            v-bind="item.badge.attributes"
          >
            {{ item.badge.text }}
          </component>
          <span class="vsm--title">
            <span v-if="item.iconP" >
              <img
                v-bind:src="'/demo/dist' + item.iconP"
                width="30"
                height="30"
              />
            </span>
            {{ item.title }}
            <div
              v-if="item.image"
              style="background-color: white; width: 53px; height: 53px;"
              @click="() => this.selectItem(item)"
            >
              <img
                v-bind:src="item.image"
                width="50"
                height="50"
              />
              <!-- <img border="0" src="./TOP.png" width="50" height="50" alt="イラスト1"> -->
            </div>
          </span>
          <div
            v-if="item.child"
            class="vsm--arrow"
            :class="[
              { 'vsm--arrow_open': show },
              { 'vsm--arrow_slot': $slots['dropdown-icon'] }
            ]"
          >
            <slot name="dropdown-icon" />
          </div>
        </template>
      </component>
    </template>
    <template v-if="item.child">
      <template v-if="(isCollapsed && !isFirstLevel) || !isCollapsed">
        <transition
          name="expand"
          @enter="expandEnter"
          @afterEnter="expandAfterEnter"
          @beforeLeave="expandBeforeLeave"
        >
          <div v-if="show" class="vsm--dropdown">
            <div class="vsm--list">
              <sidebar-menu-item
                v-for="(subItem, index) in item.child"
                :key="index"
                :item="subItem"
                :level="level + 1"
                :show-child="showChild"
                :rtl="rtl"
                :is-collapsed="isCollapsed"
              >
                <slot slot="dropdown-icon" name="dropdown-icon" />
              </sidebar-menu-item>
            </div>
          </div>
        </transition>
      </template>
    </template>
  </div>
</template>

<script>
import { itemMixin, animationMixin } from "../mixin";
export default {
  data: {
    avatar: "T1.png"
  },
  name: "SidebarMenuItem",
  mixins: [itemMixin, animationMixin],
  methods: {
    selectItem: function(item) {
      this.$store.commit("setCloth",item)
    }
  },
  props: {
    item: {
      type: Object,
      required: true
    },
    level: {
      type: Number,
      default: 1
    },
    isCollapsed: {
      type: Boolean
    },
    isMobileItem: {
      type: Boolean,
      default: false
    },
    mobileItem: {
      type: Object,
      default: null
    },
    activeShow: {
      type: Object,
      default: null
    },
    showChild: {
      type: Boolean,
      default: false
    },
    showOneChild: {
      type: Boolean,
      default: false
    },
    rtl: {
      type: Boolean,
      default: false
    },
    disableHover: {
      type: Boolean,
      default: false
    }
  }
};
</script>