<template>
  <div class="challenge-item" :class="{ 'completed': status === 'done' }">
    <div class="challenge-header">
      <h3 class="challenge-title">{{ title }}</h3>
      <div class="challenge-meta">
        <span class="challenge-status" v-if="status === 'done'">
          <i class="icon-check"></i> Đã hoàn thành
        </span>
        <span class="challenge-due-date" v-else>
          <i class="icon-clock"></i> Hạn: {{ formattedDueDate }}
        </span>
      </div>
    </div>
    
    <div class="challenge-reward">
      <span v-if="rewardType === 'points'">
        <i class="icon-star"></i> {{ rewardValue }} điểm
      </span>
      <span v-else>
        <i class="icon-gift"></i> Voucher
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChallengeItem',
  props: {
    title: {
      type: String,
      required: true
    },
    reward: {
      type: Object,
      required: true,
      validator: value => {
        return ('type' in value && ['points', 'voucher'].includes(value.type)) && 
               (value.type !== 'points' || 'value' in value)
      }
    },
    dueDate: {
      type: [String, Date],
      required: false
    },
    status: {
      type: String,
      default: 'pending',
      validator: value => ['pending', 'done'].includes(value)
    }
  },
  computed: {
    rewardType() {
      return this.reward.type
    },
    rewardValue() {
      return this.reward.value || 0
    },
    formattedDueDate() {
      if (!this.dueDate) return 'Không có hạn chót'
      
      const date = new Date(this.dueDate)
      return date.toLocaleDateString('vi-VN', {
        year: 'numeric',
        month: 'short',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      })
    }
  }
}
</script>

<style scoped>
/* Base styles */
.challenge-item {
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
  background-color: #ffffff;
  transition: all 0.3s ease;
}

.challenge-item:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-2px);
}

.challenge-item.completed {
  opacity: 0.8;
  background-color: #f8f9fa;
}

.challenge-header {
  display: flex;
  flex-direction: column;
  margin-bottom: 12px;
}

.challenge-title {
  margin: 0 0 8px 0;
  font-size: 1rem;
  font-weight: 600;
  color: #2d3748;
  line-height: 1.4;
}

.challenge-meta {
  display: flex;
  align-items: center;
}

.challenge-status,
.challenge-due-date {
  font-size: 0.875rem;
  display: inline-flex;
  align-items: center;
}

.challenge-status {
  color: #38a169;
}

.challenge-due-date {
  color: #e53e3e;
}

.challenge-reward {
  font-size: 0.875rem;
  color: #dd6b20;
  display: inline-flex;
  align-items: center;
  padding: 4px 8px;
  background-color: #fffaf0;
  border-radius: 4px;
}

/* Icons */
.icon-check,
.icon-clock,
.icon-star,
.icon-gift {
  margin-right: 6px;
  font-size: 0.9rem;
}

/* Responsive styles */
@media (min-width: 640px) {
  .challenge-header {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  
  .challenge-title {
    margin-bottom: 0;
    font-size: 1.1rem;
  }
}

@media (min-width: 768px) {
  .challenge-item {
    padding: 20px;
  }
  
  .challenge-title {
    font-size: 1.2rem;
  }
  
  .challenge-status,
  .challenge-due-date,
  .challenge-reward {
    font-size: 1rem;
  }
}

@media (min-width: 1024px) {
  .challenge-item {
    padding: 24px;
  margin-bottom: 20px;
  }
}
</style>