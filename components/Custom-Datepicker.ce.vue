<template>
  <div ref="datepickerWrapper">
    <Datepicker
      :modelValue="date"
      @update:modelValue="handleDate"
      locale="de"
      :minDate="setDate()"
      format="MMM dd, yyyy"
      :disabled="isDisabled"
      selectText="Auswählen"
      cancelText="Abbrechen"
      :enableTimePicker="false"
    >
  </Datepicker>
  <!-- <input :id="id.slice(0, -1)" :value="dateSelected()" /> -->
  </div>
</template>

<!-- <script>
export default{
  inheritAttrs: false
}
</script> -->
<script setup>
import { ref, computed, watch, useAttrs } from "vue";
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
// setting props
const props = defineProps({
  propdate: {
    type: String,
    default: new Date()
  },
  disabled: {
    type: String,
    default: 'false'
  },
  id: {
    type: String,
    default: 'deliveryTime'
  }
});
const isDisabled = computed(() => {
  return props.disabled !== 'false'
})
const datepickerWrapper = ref(null);
const font = ref("'Open Sans', sans-serif")

const setDate = () => {
  if (isNaN(props.propdate)) {
    const [day, month, year] = props.propdate.split('.');
    return new Date(+year, +month - 1, +day)
  }
  return props.propdate
}
const date = ref(setDate());
const handleDate = (modelData) => {
  date.value = modelData;
}
watch(
  () => date.value,
  (newValue, oldValue) => {
    console.log(newValue)
    const value = dateSelected(newValue)
    const dateVal = new CustomEvent("selected", {
      bubbles: true,
      composed: true,
      detail: { val: value }
    })
    datepickerWrapper.value.dispatchEvent(dateVal);
  }
);

const dateSelected = (e) => {
  if (isNaN(e)) {
    console.log(0)
    let dateFormat = new Intl.DateTimeFormat('en', { month: 'short', day: '2-digit', year: 'numeric' })
    return dateFormat.format(e)
  }
  return e
}
// defineExpose({
//   date
// })
// creating & emitting events
const emit = defineEmits(["selected"]);
</script>
<style lang="scss">
// General
$dp__font_family: "Open Sans", sans-serif !default; // Font size for the menu
//$dp__font_family: -apple-system, blinkmacsystemfont, "Segoe UI", roboto, oxygen, ubuntu, cantarell, "Open Sans", "Helvetica Neue", sans-serif !default; // Font size for the menu
$dp__border_radius: 4px !default; // Border radius everywhere
$dp__cell_border_radius: $dp__border_radius !default; // Specific border radius for the calendar cell

// Transitions
$dp__transition_length: 22px !default; // Passed to the translateX in animation
$dp__transition_duration: 0.1s !default; // Transition duration

// Sizing
$dp__button_height: 35px !default; // size for buttons in overlays
$dp__month_year_row_height: 35px !default; // height of the month-year select row
$dp__month_year_row_button_size: 25px !default; // Specific height for the next/previous buttons
$dp__button_icon_height: 20px !default; // icon sizing in buttons
$dp__cell_size: 35px !default; // width and height of calendar cell
$dp__cell_padding: 5px !default; // padding in the cell
$dp__common_padding: 10px !default;
$dp__input_padding: 6px 12px !default; // padding in the input
$dp__input_icon_padding: 35px !default; // Padding on the left side of the input if icon is present
$dp__menu_min_width: 260px !default; // Adjust the min width of the menu
$dp__action_buttons_padding: 2px 5px !default; // Adjust padding for the action buttons in action row
$dp__row_margin: 5px 0 !default; // Adjust the spacing between rows in the calendar
$dp__calendar_header_cell_padding: 0.5rem !default; // Adjust padding in calendar header cells
$dp__two_calendars_spacing: 10px !default; // Space between two calendars if using two calendars
$dp__overlay_col_padding: 3px !default; // Padding in the overlay column
$dp__time_inc_dec_button_size: 32px !default; // Sizing for arrow buttons in the time picker

// Font sizes
$dp__font_size: 1rem !default; // overall font-size
$dp__preview_font_size: 0.8rem !default; // font size of the date preview in the action row
$dp__time_font_size: 2rem !default; // font size in the time picker


.dp__theme_dark {
  --dp-background-color: #212121;
  --dp-text-color: #fff;
  --dp-hover-color: #484848;
  --dp-hover-text-color: #fff;
  --dp-hover-icon-color: #959595;
  --dp-primary-color: #005cb2;
  --dp-primary-disabled-color: #61a8ea;
  --dp-primary-text-color: #fff;
  --dp-secondary-color: #a9a9a9;
  --dp-border-color: #2d2d2d;
  --dp-menu-border-color: #2d2d2d;
  --dp-border-color-hover: #aaaeb7;
  --dp-disabled-color: #737373;
  --dp-disabled-color-text: #d0d0d0;
  --dp-scroll-bar-background: #212121;
  --dp-scroll-bar-color: #484848;
  --dp-success-color: #00701a;
  --dp-success-color-disabled: #428f59;
  --dp-icon-color: #959595;
  --dp-danger-color: #e53935;
  --dp-marker-color: #e53935;
  --dp-tooltip-color: #3e3e3e;
  --dp-highlight-color: rgb(0 92 178 / 20%);
}

.dp__theme_light {
  --dp-background-color: #fff;
  --dp-text-color: #212121;
  --dp-hover-color: #f3f3f3;
  --dp-hover-text-color: #212121;
  --dp-hover-icon-color: #959595;
  --dp-primary-color: #1976d2;
  --dp-primary-disabled-color: #6bacea;
  --dp-primary-text-color: #f8f5f5;
  --dp-secondary-color: #c0c4cc;
  --dp-border-color: #ddd;
  --dp-menu-border-color: #ddd;
  --dp-border-color-hover: #aaaeb7;
  --dp-disabled-color: #f6f6f6;
  --dp-scroll-bar-background: #f3f3f3;
  --dp-scroll-bar-color: #959595;
  --dp-success-color: #76d275;
  --dp-success-color-disabled: #a3d9b1;
  --dp-icon-color: #959595;
  --dp-danger-color: #ff6f60;
  --dp-marker-color: #ff6f60;
  --dp-tooltip-color: #fafafa;
  --dp-disabled-color-text: #8e8e8e;
  --dp-highlight-color: rgb(25 118 210 / 10%);
}

.dp__main {
  font-family: v-bind(font);
  //font-family: $dp__font_family;
  user-select: none;
  box-sizing: border-box;
}

.dp__pointer {
  cursor: pointer;
}

// Allow control of color property on icons
.dp__icon {
  stroke: currentcolor;
  fill: currentcolor;
}

// Div style that is displayed on the bottom of the menu for switching layouts
.dp__button {
  width: 100%;
  text-align: center;
  color: var(--dp-icon-color);
  background: var(--dp-background-color);
  cursor: pointer;
  display: flex;
  align-items: center;
  align-content: center;
  justify-content: center;
  padding: $dp__common_padding;
  box-sizing: border-box;
  height: $dp__button_height;
// added to hide time button
  display: none !important;

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-hover-icon-color);
  }

  svg {
    height: $dp__button_icon_height;
    width: auto;
  }
}

.dp__button_bottom {
  border-bottom-left-radius: $dp__border_radius;
  border-bottom-right-radius: $dp__border_radius;
}

.dp__flex_display {
  display: flex;
}

.dp__flex_display_with_input {
  flex-direction: column;
  align-items: start;
}

.dp__relative {
  position: relative;
}

.calendar-next-enter-active,
.calendar-next-leave-active,
.calendar-prev-enter-active,
.calendar-prev-leave-active {
  transition: all $dp__transition_duration ease-out;
}

.calendar-next-enter-from {
  opacity: 0;
  transform: translateX($dp__transition_length);
}

.calendar-next-leave-to {
  opacity: 0;
  transform: translateX(-$dp__transition_length);
}

.calendar-prev-enter-from {
  opacity: 0;
  transform: translateX(-$dp__transition_length);
}

.calendar-prev-leave-to {
  opacity: 0;
  transform: translateX($dp__transition_length);
}

.dp-menu-appear-enter-active,
.dp-menu-appear-leave-active,
.dp-slide-up-enter-active,
.dp-slide-up-leave-active,
.dp-slide-down-enter-active,
.dp-slide-down-leave-active {
  transition: all 0.1s ease-out;
}

.dp-slide-down-leave-to,
.dp-slide-up-enter-from {
  opacity: 0;
  transform: translateY($dp__transition_length);
}

.dp-slide-down-enter-from,
.dp-slide-up-leave-to {
  opacity: 0;
  transform: translateY(-$dp__transition_length);
}

.dp-menu-appear-enter-from {
  opacity: 0;
}

.dp-menu-appear-leave-to {
  opacity: 1;
}


.dp__input_wrap {
  position: relative;
  width: 100%;
  box-sizing: unset;

  &:focus {
    border-color: var(--dp-border-color-hover);
    outline: none;
  }
}

.dp__input {
  background-color: var(--dp-background-color);
  border-radius: $dp__border_radius;
  //font-family: $dp__font_family;
  font-family: v-bind(font);
  border: 1px solid var(--dp-border-color);
  outline: none;
  transition: border-color 0.2s cubic-bezier(0.645, 0.045, 0.355, 1);
  width: 100%;
  font-size: $dp__font_size;
  line-height: $dp__font_size * 1.5;
  padding: $dp__input_padding;
  color: var(--dp-text-color);
  box-sizing: border-box;

  &::placeholder {
    opacity: 0.7;
  }

  &:hover {
    border-color: var(--dp-border-color-hover);
  }
}

.dp__input_reg {
  caret-color: transparent;
}

.dp__input_focus {
  border-color: var(--dp-border-color-hover);
}

.dp__disabled {
  background: var(--dp-disabled-color);

  &::placeholder {
    color: var(--dp-disabled-color-text);
  }
}

.dp__input_icons {
  display: inline-block;
  width: $dp__font_size;
  height: $dp__font_size;
  stroke-width: 0;
  font-size: $dp__font_size;
  line-height: $dp__font_size * 1.5;
  padding: $dp__input_padding;
  color: var(--dp-icon-color);
  box-sizing: content-box;
}

.dp__input_icon {
  cursor: pointer;
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  color: var(--dp-icon-color);
}

.dp__clear_icon {
  position: absolute;
  top: 50%;
  right: 0;
  transform: translateY(-50%);
  cursor: pointer;
  color: var(--dp-icon-color);
}

.dp__input_icon_pad {
  padding-left: $dp__input_icon_padding;
}

.dp__input_valid {
  box-shadow: 0 0 $dp__border_radius var(--dp-success-color);
  border-color: var(--dp-success-color);

  &:hover {
    border-color: var(--dp-success-color);
  }
}

.dp__input_invalid {
  box-shadow: 0 0 $dp__border_radius var(--dp-danger-color);
  border-color: var(--dp-danger-color);

  &:hover {
    border-color: var(--dp-danger-color);
  }
}


.dp__menu {
  position: absolute;
  background: var(--dp-background-color);
  border-radius: $dp__border_radius;
  min-width: $dp__menu_min_width;
  font-family: v-bind(font);
  //font-family: $dp__font_family;
  font-size: $dp__font_size;
  user-select: none;
  border: 1px solid var(--dp-menu-border-color);
  box-sizing: border-box;

  &::after {
    box-sizing: border-box;
  }

  &::before {
    box-sizing: border-box;
  }

  &:focus {
    border: 1px solid var(--dp-menu-border-color);
    outline: none;
  }
}

.dp__menu_index {
  z-index: 99999;
}

%__dp_menu_readonly_disabled {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
}

.dp__menu_disabled {
  @extend %__dp_menu_readonly_disabled;

  background: rgb(255 255 255 / 50%);
  cursor: not-allowed;
}

.dp__menu_readonly {
  @extend %__dp_menu_readonly_disabled;

  background: transparent;
  cursor: default;
}

.dp__arrow_top {
  left: 50%;
  top: -1px;
  height: 12px;
  width: 12px;
  background-color: var(--dp-background-color);
  position: absolute;
  border-left: 1px solid var(--dp-menu-border-color);
  border-top: 1px solid var(--dp-menu-border-color);
  transform: translate(-50%, -50%) rotate(45deg);
}

.dp__arrow_bottom {
  left: 50%;
  bottom: -1px;
  height: 12px;
  width: 12px;
  background-color: var(--dp-background-color);
  position: absolute;
  border-right: 1px solid var(--dp-menu-border-color);
  border-bottom: 1px solid var(--dp-menu-border-color);
  transform: translate(-50%, 50%) rotate(45deg);
}

.dp__now_wrap {
  text-align: center;
  padding: 2px 0;
}

.dp__now_button {
  border: 1px solid var(--dp-primary-color);
  color: var(--dp-primary-color);
  padding: 0 4px;
  font-weight: bold;
  border-radius: $dp__border_radius;
  font-size: $dp__font_size;
  cursor: pointer;
  background: transparent;
}

.dp__preset_ranges {
  padding: 5px;
  border-right: 1px solid var(--dp-border-color);
}

.dp__sidebar_left {
  padding: 5px;
  border-right: 1px solid var(--dp-border-color);
}

.dp__sidebar_right {
  padding: 5px;
  border-left: 1px solid var(--dp-border-color);
}

.dp__preset_range {
  padding: 5px;

  &:hover {
    background-color: var(--dp-hover-color);
    cursor: pointer;
  }
}

.dp__menu_content_wrapper {
  display: flex;
}


.dp__calendar_wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: v-bind(font);
  //font-family: $dp__font_family;
  flex: 0;
}

.dp__calendar_header {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--dp-text-color);
  white-space: nowrap;
  font-weight: bold;
}

.dp__calendar_header_item {
  text-align: center;
  flex-grow: 1;
  height: $dp__cell_size;
  padding: $dp__cell_padding;
  width: $dp__cell_size;
  box-sizing: border-box;
}

.dp__calendar_row {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: $dp__row_margin;
}

.dp__calendar_item {
  text-align: center;
  flex-grow: 1;
  box-sizing: border-box;
  color: var(--dp-text-color);
}

.dp__calendar {
  position: relative;
}

.dp__calendar_header_cell {
  border-bottom: thin solid var(--dp-border-color);
  padding: $dp__calendar_header_cell_padding;
}

.dp__cell_inner {
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  border-radius: $dp__cell_border_radius;
  height: $dp__cell_size;
  padding: $dp__cell_padding;
  width: $dp__cell_size;
  border: 1px solid transparent;
  box-sizing: border-box;
  position: relative;
}

%__dp_range_border_radius_start {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}

%__dp_range_border_radius_end {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}

%__dp_active {
  background: var(--dp-primary-color);
  color: var(--dp-primary-text-color);
}

%__dp_range_preview {
  border-top: 1px dashed var(--dp-primary-color);
  border-bottom: 1px dashed var(--dp-primary-color);
}

%__hover_date {
  background: var(--dp-hover-color);
  color: var(--dp-hover-text-color);
}

.dp__cell_offset {
  color: var(--dp-secondary-color);
}

.dp__cell_disabled {
  color: var(--dp-secondary-color);
  cursor: not-allowed;
}

.dp__active_date {
  @extend %__dp_active;
}

.dp__range_start {
  @extend %__dp_active;
  @extend %__dp_range_border_radius_start;
}

.dp__range_end {
  @extend %__dp_active;
  @extend %__dp_range_border_radius_end;
}

.dp__date_hover {
  &:hover {
    @extend %__hover_date;
  }
}

.dp__date_hover_start {
  &:hover {
    @extend %__dp_range_border_radius_start;
    @extend %__hover_date;
  }
}

.dp__date_hover_end {
  &:hover {
    @extend %__dp_range_border_radius_end;
    @extend %__hover_date;
  }
}

.dp__range_between {
  background: var(--dp-hover-color);
  border-radius: 0;
  border-top: 1px solid var(--dp-hover-color);
  border-bottom: 1px solid var(--dp-hover-color);
}

.dp__range_between_week {
  background: var(--dp-primary-color);
  color: var(--dp-primary-text-color);
  border-radius: 0;
  border-top: 1px solid var(--dp-primary-color);
  border-bottom: 1px solid var(--dp-primary-color);
}

.dp__today {
  border: 1px solid var(--dp-primary-color);
}

.dp__week_num {
  color: var(--dp-secondary-color);
  text-align: center;
}

.dp__cell_auto_range {
  border-radius: 0;
  border-top: 1px dashed var(--dp-primary-color);
  border-bottom: 1px dashed var(--dp-primary-color);
}

.dp__cell_auto_range_start {
  @extend %__dp_range_border_radius_start;
  @extend %__dp_range_preview;

  border-left: 1px dashed var(--dp-primary-color);
}

.dp__cell_auto_range_end {
  @extend %__dp_range_border_radius_end;
  @extend %__dp_range_preview;

  border-right: 1px dashed var(--dp-primary-color);
}

.dp__calendar_header_separator {
  width: 100%;
  height: 1px;
  background: var(--dp-border-color);
}

.dp__calendar_next {
  margin-left: $dp__two_calendars_spacing;
}

%__dp_marker {
  height: 5px;
  background-color: var(--dp-marker-color);
  position: absolute;
  bottom: 0;
}

.dp__marker_dot {
  width: 5px;
  border-radius: 50%;
  left: 50%;
  transform: translateX(-50%);

  @extend %__dp_marker;
}

.dp__marker_line {
  width: 100%;
  left: 0;

  @extend %__dp_marker;
}

.dp__marker_tooltip {
  position: absolute;
  border-radius: $dp__border_radius;
  background-color: var(--dp-tooltip-color);
  padding: 5px;
  border: 1px solid var(--dp-border-color);
  z-index: 99999;
  box-sizing: border-box;
  cursor: default;
}

.dp__tooltip_content {
  white-space: nowrap;
}

.dp__tooltip_text {
  display: flex;
  align-items: center;
  flex-flow: row nowrap;
  color: var(--dp-text-color);
}

.dp__tooltip_mark {
  height: 5px;
  width: 5px;
  border-radius: 50%;
  background-color: var(--dp-text-color);
  color: var(--dp-text-color);
  margin-right: 5px;
}

.dp__arrow_bottom_tp {
  left: 50%;
  bottom: 0;
  height: 8px;
  width: 8px;
  background-color: var(--dp-tooltip-color);
  position: absolute;
  border-right: 1px solid var(--dp-border-color);
  border-bottom: 1px solid var(--dp-border-color);
  transform: translate(-50%, 50%) rotate(45deg);
}

.dp__instance_calendar {
  position: relative;
}

@media only screen and (max-width: 600px) {
  .dp__flex_display {
    flex-direction: column;
  }
}

.dp__cell_highlight {
  background-color: var(--dp-highlight-color);
}


.dp__month_year_row {
  display: flex;
  align-items: center;
  height: $dp__month_year_row_height;
  color: var(--dp-text-color);
  box-sizing: border-box;
}

.dp__inner_nav {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  height: $dp__month_year_row_button_size;
  width: $dp__month_year_row_button_size;
  color: var(--dp-icon-color);
  text-align: center;
  border-radius: 50%;

  svg {
    height: $dp__button_icon_height;
    width: $dp__button_icon_height;
  }

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-hover-icon-color);
  }
}

%__dp_inner_nav_disabled {
  background: var(--dp-disabled-color);
  color: var(--dp-disabled-color-text);
  cursor: not-allowed;
}

.dp__inner_nav_disabled {
  @extend %__dp_inner_nav_disabled;

  &:hover {
    @extend %__dp_inner_nav_disabled;
  }
}

.dp__month_year_select {
  width: 50%;
  text-align: center;
  cursor: pointer;
  height: $dp__month_year_row_height;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: $dp__border_radius;
  box-sizing: border-box;

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-hover-text-color);
  }
}


.dp__overlay {
  position: absolute;
  overflow-y: auto;
  width: 100%;
  height: 100%;
  background: var(--dp-background-color);
  top: 0;
  left: 0;
  transition: opacity 1s ease-out;
  z-index: 99999;
  font-family: v-bind(font);
  //font-family: $dp__font_family;
  color: var(--dp-text-color);
  box-sizing: border-box;
}

.dp__overlay::-webkit-scrollbar-track {
  box-shadow: var(--dp-scroll-bar-background);
  background-color: var(--dp-scroll-bar-background);
}

.dp__overlay::-webkit-scrollbar {
  width: 5px;
  background-color: var(--dp-scroll-bar-background);
}

.dp__overlay::-webkit-scrollbar-thumb {
  background-color: var(--dp-scroll-bar-color);
  border-radius: 10px;
}

.dp__overlay:focus {
  border: none;
  outline: none;
}

.dp__container_flex {
  display: flex;
}

.dp__container_block {
  display: block;
}

.dp__overlay_container {
  height: 100%;
  flex-direction: column;
}

.dp__overlay_row {
  padding: 0;
  box-sizing: border-box;
  display: flex;
  margin-left: auto;
  margin-right: auto;
  flex-wrap: wrap;
  max-width: 100%;
  width: 100%;
  align-items: center;
}

.dp__overlay_container > .dp__overlay_row {
  flex: 1;
}

.dp__overlay_col {
  box-sizing: border-box;
  width: 33%;
  padding: $dp__overlay_col_padding;
  white-space: nowrap;
}

.dp__overlay_cell_pad {
  padding: $dp__common_padding 0;
}

.dp__overlay_cell_active {
  cursor: pointer;
  border-radius: $dp__border_radius;
  text-align: center;
  background: var(--dp-primary-color);
  color: var(--dp-primary-text-color);
}

.dp__overlay_cell {
  cursor: pointer;
  border-radius: $dp__border_radius;
  text-align: center;

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-hover-text-color);
  }
}

.dp__cell_in_between {
  background: var(--dp-hover-color);
  color: var(--dp-hover-text-color);
}

.dp__overlay_action {
  position: sticky;
  bottom: 0;
  background: #fff;
}

.dp__over_action_scroll {
  right: 5px;
  box-sizing: border-box;
}

.dp__overlay_cell_disabled {
  cursor: not-allowed;
  background: var(--dp-disabled-color);

  &:hover {
    background: var(--dp-disabled-color);
  }
}

.dp__overlay_cell_active_disabled {
  cursor: not-allowed;
  background: var(--dp-primary-disabled-color);

  &:hover {
    background: var(--dp-primary-disabled-color);
  }
}

.dp__month_picker_header {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  height: $dp__cell_size;
}


.dp__time_input {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none;
  font-family: v-bind(font);
  //font-family: $dp__font_family;
  color: var(--dp-text-color);
}

.dp__time_col_reg {
  padding: 0 20px;
}

.dp__time_col_reg_with_button {
  padding: 0 15px;
}

.dp__time_col_sec {
  padding: 0 10px;
}

.dp__time_col_sec_with_button {
  padding: 0 5px;
}

.dp__time_col {
  font-size: $dp__time_font_size;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.dp__time_display {
  cursor: pointer;
  color: var(--dp-text-color);
  border-radius: $dp__border_radius;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 3px;

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-hover-text-color);
  }
}

.dp__inc_dec_button {
  padding: 5px;
  margin: 0;
  height: $dp__time_inc_dec_button_size;
  width: $dp__time_inc_dec_button_size;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 50%;
  color: var(--dp-icon-color);
  box-sizing: border-box;

  svg {
    height: $dp__time_inc_dec_button_size;
    width: $dp__time_inc_dec_button_size;
  }

  &:hover {
    background: var(--dp-hover-color);
    color: var(--dp-primary-color);
  }
}

.dp__pm_am_button {
  background: var(--dp-primary-color);
  color: var(--dp-primary-text-color);
  border: none;
  padding: $dp__common_padding;
  border-radius: $dp__border_radius;
  cursor: pointer;
}


.dp__action_row {
  display: flex;
  align-items: center;
  width: 100%;
  padding: $dp__common_padding;
  box-sizing: border-box;
  color: var(--dp-text-color);
  background: var(--dp-background-color);

  svg {
    height: $dp__button_icon_height;
    width: auto;
  }
}

.dp__selection_preview {
  width: 50%;
  color: var(--dp-text-color);
  font-size: $dp__preview_font_size;
}

.dp__action_buttons {
  width: 50%;
  text-align: right;
}

.dp__action {
  font-weight: bold;
  cursor: pointer;
  padding: $dp__action_buttons_padding;
  border-radius: $dp__border_radius;
  display: inline-flex;
  align-items: center;
}

.dp__select {
  color: var(--dp-success-color);
}

.dp__action_disabled {
  color: var(--dp-success-color-disabled);
  cursor: not-allowed;
}

.dp__cancel {
  color: var(--dp-secondary-color);
}

</style>