<template>
    <div>
        <Common>
            <ul class="timeline-wrapper">
                <li
                    v-for="(item, index) in $articalsForTimeline"
                    :key="index"
                >
                    <h3 class="year">
                        {{ item.year }}
                    </h3>
                    <ul class="year-wrapper">
                        <li
                            v-for="(subItem, subIndex) in item.data"
                            :key="subIndex"
                        >
                            <span class="date">{{ subItem.frontmatter.date | dateFormat }}</span>
                            <span
                                class="title"
                                @click="go(subItem.path)"
                            >{{ subItem.title }}</span>
                        </li>
                    </ul>
                </li>
            </ul>
        </Common>
    </div>
</template>

<script>
import Common from '@theme/components/Common';

export default {
    name: 'time-line',
    components: { Common },
    filters: {
        dateFormat(date, type) {
            function renderTime(date) {
                const dateee = new Date(date).toJSON();
                return new Date(+new Date(dateee) + 8 * 3600 * 1000).toISOString().replace(/T/g, ' ').replace(/\.[\d]{3}Z/, '').replace(/-/g, '/');
            }
            date = renderTime(date);
            const dateObj = new Date(date);
            const mon = dateObj.getMonth() + 1;
            const day = dateObj.getDate();
            return `${mon}-${day}`;
        }
    },
    methods: {
        go(url) {
            this.$router.push({ path: url });
        }
    }
};
</script>

<style lang="stylus" scoped>
// @require '../styles/wrapper.styl'

.timeline-wrapper
  box-sizing border-box
  max-width: 740px;
  margin: 8rem auto 4rem;
  position relative
  list-style none
  &::after {
    content: " ";
    position: absolute;
    top: 14px;
    left: 0;
    z-index: 0;
    margin-left: -2px;
    width: 4px;
    height: 100%;
    background: #eaecef;
  }
  .desc, .year {
    position: relative;
    color: #000;
    font-size 16px
    &:before {
      content: " ";
      position: absolute;
      z-index 2;
      left: -20px;
      top: 50%;
      margin-left: -4px;
      margin-top: -4px;
      width: 8px;
      height: 8px;
      background: #fff;
      border: 1px solid #eaecef;
      border-radius: 50%;
    }
  }
  .year {
    margin: 80px 0 0px;
    font-weight: 700;
    font-size 26px
  }
  .year-wrapper {
    padding-left 0!important
    li {
      display flex
      padding 20px 0 10px
      list-style none
      border-bottom: 1px dashed #eaecef;
      position relative
      &:hover {
        .date {
          color $accentColor
          &::before {
            background $accentColor
          }
        }
        .title {
          color $accentColor
        }
      }
      .date {
        width 40px
        line-height 30px
        color $textColorSub
        font-size 12px
        &::before {
          content: " ";
          position: absolute;
          left: -19px;
          top: 31px;
          width: 6px;
          height: 6px;
          margin-left: -4px;
          background: #fff;
          border-radius: 50%;
          border: 1px solid #eaecef;
          z-index 2
        }
      }
      .title {
        line-height 30px
        color $textColorSub
        font-size 16px
        cursor pointer
      }
    }
  }
@media (max-width: $MQMobile)
  .timeline-wrapper
    margin: 0 1.2rem;

</style>
