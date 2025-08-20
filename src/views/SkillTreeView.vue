&lt;template&gt;
  &lt;div class="skill-temple"&gt;
    &lt;div class="skill-temple__container"&gt;
      &lt;div class="skill-temple__rotate-container" ref="rotateContainer"&gt;
        &lt;div class="skill-temple__tree"&gt;
          &lt;!-- 技能节点将在这里动态生成 --&gt;
        &lt;/div&gt;
      &lt;/div&gt;
      &lt;div class="skill-temple__details" v-if="selectedSkill"&gt;
        &lt;div class="skill-temple__card pixel-card"&gt;
          &lt;h3&gt;{{ selectedSkill.name }}&lt;/h3&gt;
          &lt;div class="skill-temple__level"&gt;
            &lt;div class="progress-bar"&gt;
              &lt;div :style="{ width: `${selectedSkill.level}%` }" class="progress"&gt;&lt;/div&gt;
            &lt;/div&gt;
            &lt;span class="level-text"&gt;LV.{{ Math.floor(selectedSkill.level / 10) }}&lt;/span&gt;
          &lt;/div&gt;
          &lt;p&gt;{{ selectedSkill.description }}&lt;/p&gt;
          &lt;div class="skill-temple__examples"&gt;
            &lt;h4&gt;使用案例：&lt;/h4&gt;
            &lt;ul&gt;
              &lt;li v-for="example in selectedSkill.examples" :key="example"&gt;{{ example }}&lt;/li&gt;
            &lt;/ul&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;
    &lt;div class="skill-temple__categories"&gt;
      &lt;button 
        v-for="category in categories" 
        :key="category.id"
        :class="['pixel-button', { active: currentCategory === category.id }]"
        @click="selectCategory(category.id)"
      &gt;
        {{ category.name }}
      &lt;/button&gt;
    &lt;/div&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
import { ref, onMounted } from 'vue'

const rotateContainer = ref(null)
const selectedSkill = ref(null)
const currentCategory = ref('frontend')

// 技能分类
const categories = [
  { id: 'frontend', name: '前端技能' },
  { id: 'backend', name: '后端技能' },
  { id: 'design', name: '设计技能' },
  { id: 'soft', name: '软技能' }
]

// 技能数据（示例）
const skills = {
  frontend: [
    {
      id: 'vue',
      name: 'Vue.js',
      level: 85,
      description: '现代化的渐进式JavaScript框架',
      examples: [
        '开发响应式单页应用',
        '构建可复用的组件系统',
        '状态管理与路由控制'
      ]
    },
    // 更多技能...
  ]
}

const selectCategory = (categoryId) => {
  currentCategory.value = categoryId
  // TODO: 切换分类时的动画效果
}

onMounted(() => {
  // 初始化3D旋转效果
  if (rotateContainer.value) {
    let isDragging = false
    let startX = 0
    let startY = 0
    let rotateX = 0
    let rotateY = 0

    rotateContainer.value.addEventListener('mousedown', (e) => {
      isDragging = true
      startX = e.clientX - rotateY
      startY = e.clientY - rotateX
    })

    document.addEventListener('mousemove', (e) => {
      if (!isDragging) return
      rotateY = e.clientX - startX
      rotateX = e.clientY - startY
      rotateContainer.value.style.transform = 
        `rotateX(${rotateX * 0.5}deg) rotateY(${rotateY * 0.5}deg)`
    })

    document.addEventListener('mouseup', () => {
      isDragging = false
    })
  }
})
&lt;/script&gt;

&lt;style scoped&gt;
.skill-temple {
  width: 100%;
  height: 100vh;
  background-color: var(--deep-bg-color, #0a0a1a);
  color: var(--text-color, #f1f1f1);
  display: flex;
  flex-direction: column;
  padding: 2rem;
}

.skill-temple__container {
  flex: 1;
  display: flex;
  gap: 2rem;
  perspective: 1000px;
}

.skill-temple__rotate-container {
  flex: 2;
  transform-style: preserve-3d;
  cursor: grab;
  transition: transform 0.1s ease;
}

.skill-temple__rotate-container:active {
  cursor: grabbing;
}

.skill-temple__tree {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
}

.skill-temple__details {
  flex: 1;
  padding: 1rem;
}

.pixel-card {
  background-color: var(--content-color, #1a1a2e);
  border: 2px solid var(--accent-color, #e94560);
  padding: 1rem;
  image-rendering: pixelated;
}

.skill-temple__level {
  margin: 1rem 0;
}

.progress-bar {
  width: 100%;
  height: 20px;
  background-color: #2a2a3e;
  border: 2px solid var(--accent-color, #e94560);
}

.progress {
  height: 100%;
  background-color: var(--accent-color, #e94560);
  transition: width 0.3s ease;
}

.level-text {
  font-family: 'Press Start 2P', monospace;
  font-size: 0.8rem;
  margin-top: 0.5rem;
  display: block;
}

.skill-temple__categories {
  display: flex;
  gap: 1rem;
  justify-content: center;
  padding: 1rem;
}

.pixel-button {
  background-color: var(--content-color, #1a1a2e);
  border: 2px solid var(--accent-color, #e94560);
  color: var(--text-color, #f1f1f1);
  padding: 0.5rem 1rem;
  font-family: 'Press Start 2P', monospace;
  font-size: 0.8rem;
  cursor: pointer;
  transition: all 0.2s ease;
  image-rendering: pixelated;
}

.pixel-button:hover {
  background-color: var(--accent-color, #e94560);
}

.pixel-button.active {
  background-color: var(--accent-color, #e94560);
  transform: scale(1.1);
}

.skill-temple__examples {
  margin-top: 1rem;
}

.skill-temple__examples ul {
  list-style: none;
  padding-left: 1rem;
}

.skill-temple__examples li {
  margin: 0.5rem 0;
  position: relative;
}

.skill-temple__examples li::before {
  content: '→';
  position: absolute;
  left: -1rem;
  color: var(--accent-color, #e94560);
}
&lt;/style&gt;
