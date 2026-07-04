<script setup lang="ts">
import { computed } from 'vue'

/**
 * lowcode 平台能力演进页组件。
 *
 * @author xiye
 * @date 2026-07-04
 * @since 1.0.0
 */
const props = defineProps<{
  clicks?: number
}>()

/**
 * Slidev 当前页点击次数。
 * 由于已经取消 click0 的独立引导态，这里直接从第 1 组能力点开始展示。
 */
const currentStep = computed(() => {
  const value = props.clicks ?? 0
  return Math.min(Math.max(value, 0), 5)
})

/**
 * 当前页签信息，用于顶部步骤高亮。
 */
const tabs = [
  '画布',
  '数据模型',
  '路由',
  'DAG',
  'API',
]

/**
 * 每一步对应的标题、说明和截图资源。
 * 注意：这里只保留 5 组正式能力点，clicks=0 的引导态单独处理。
 */
const steps = [
  {
    tag: '核心能力 01',
    title: '画布与物料系统',
    description:
      '画布是 lowcode engine 的核心入口，负责连接物料系统、数据模型、路由配置、DAG 流程与 API 配置，保证所见即所得。',
    image: '/lowcode-4.png',
    alt: '画布主页',
  },
  {
    tag: '核心能力 02',
    title: '数据模型',
    description:
      '通过定义数据库表、字段与关系，沉淀业务数据结构，作为页面编排、流程执行与系统交付的基础。',
    image: '/lowcode-1.png',
    alt: '数据模型',
  },
  {
    tag: '核心能力 03',
    title: '路由与多页面',
    description:
      '允许定义路由和多页面跳转，这是 evelan-lowcode 与玩具型平台拉开差距的关键一步，也是系统级平台能力的起点。',
    image: '/lowcode-2.png',
    alt: '路由配置',
  },
  {
    tag: '核心能力 04',
    title: 'DAG 流程配置',
    description:
      '将查询数据库、调用 API 与业务处理流程纳入统一编排，是平台从页面生成走向业务执行的第二个关键台阶。',
    image: '/lowcode-3.png',
    alt: 'DAG流程配置',
  },
  {
    tag: '核心能力 05',
    title: 'API 配置',
    description:
      '通过 API 配置对接外部系统或前后端服务，让平台具备真实业务环境下的集成与交付能力。',
    image: '/lowcode-5.png',
    alt: 'API配置',
  },
]

/**
 * 当前步骤的数据对象。
 * 当前页仍然可能产生 0 到 5 的点击值，这里统一钳制到最后一个有效步骤，
 * 避免页级 clicks 数与步骤数暂时不一致时出现空白内容。
 */
const activeIndex = computed(() => Math.min(currentStep.value, steps.length - 1))

/**
 * 当前激活的步骤数据。
 */
const activeStep = computed(() => steps[activeIndex.value])
</script>

<template>
  <div class="grid grid-cols-[0.92fr,1.08fr] gap-4 pt-2 items-start">
    <div class="space-y-3">

      <div class="rounded-xl bg-white/10 p-1 min-h-[170px]">
        <div class="text-xs uppercase tracking-widest opacity-60">{{ activeStep.tag }}</div>
        <div class="text-lg font-semibold mt-1.5 leading-7">{{ activeStep.title }}</div>
        <p class="text-[13px] leading-6 mt-2 opacity-90">
          {{ activeStep.description }}
        </p>
      </div>
    </div>

    <div class="rounded-2xl bg-black/20 mt-[-80px] min-h-[340px] flex items-center justify-center overflow-hidden">
      <img :src="activeStep.image" :alt="activeStep.alt" class="max-h-[300px] w-auto rounded-xl m-auto object-contain">
    </div>
  </div>
</template>
