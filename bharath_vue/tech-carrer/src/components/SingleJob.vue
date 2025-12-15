<template>
  <div :class="['job-card', job.SalaryType === 'Paid' ? 'paid' : 'unpaid']">
    <!-- HEADER -->
    <div class="job-header">
      <img
        v-if="job.CompanyLogo"
        :src="job.CompanyLogo"
        alt="Company logo"
        class="company-logo"
      />
      <div class="job-header-text">
        <h2 class="company-name">{{ job.CompanyName }}</h2>
        <h3 class="job-title">{{ job.JobTitle }}</h3>
        <p class="job-type">{{ job.JobType }} - {{ job.Location }}</p>
      </div>
    </div>

    <!-- SALARY -->
    <p class="salary">
      {{ job.SalaryType }} - {{ job.EstimatedSalary }}
    </p>

    <!-- DESCRIPTION -->
    <p class="description">{{ job.JobDescription }}</p>

    <!-- FOOTER -->
    <div class="card-footer">
      <a
        :href="job.ApplyLink"
        target="_blank"
        class="apply-button"
        aria-label="Apply for this job"
      >
        Apply Now
      </a>

      <button
        class="bookmark-btn"
        @click="toggleBookmark"
        :class="{ bookmarked: isBookmarked }"
        aria-label="Bookmark this job"
      >
        {{ isBookmarked ? "★ Bookmarked" : "☆ Bookmark" }}
      </button>

      <div v-if="job.IsRemote" class="remote-tag">Remote</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleJob",
  props: {
    job: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isBookmarked: false
    };
  },
  async mounted() {
    // Check if this job is already bookmarked
    try {
      const res = await fetch(
        "https://new-node-production.up.railway.app/bookmarks"
      );
      const bookmarks = await res.json();
      this.isBookmarked = bookmarks.some(
        b => b.job_id === this.job.job_id
      );
    } catch (err) {
      console.error("Failed to check bookmark status:", err);
    }
  },
  methods: {
    async toggleBookmark() {
      try {
        const payload = {
          job_id: this.job.job_id || `${this.job.CompanyName}-${this.job.JobTitle}`.replace(/\s+/g, "_").toLowerCase(),
          CompanyName: this.job.CompanyName,
          JobTitle: this.job.JobTitle,
          JobType: this.job.JobType,
          Location: this.job.Location,
          SalaryType: this.job.SalaryType,
          EstimatedSalary: this.job.EstimatedSalary,
          JobDescription: this.job.JobDescription,
          ApplyLink: this.job.ApplyLink || "",
          IsRemote: this.job.IsRemote || false
        };

        const res = await fetch(
          "https://new-node-production.up.railway.app/bookmarks",
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(payload)
          }
        );

        const data = await res.json();

        if (!res.ok) {
          alert(data.error || "Bookmark failed");
          return;
        }

        // Toggle local state
        this.isBookmarked = data.action === "added";

        alert(
          data.action === "added"
            ? "✅ Job bookmarked"
            : "❌ Bookmark removed"
        );
      } catch (err) {
        console.error("Bookmark error:", err);
        alert("❌ Error bookmarking job");
      }
    }
  }
};
</script>

<style scoped>
/* ---------------- CARD ---------------- */
.job-card {
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  position: relative;
  display: grid;
  grid-template-rows: auto auto auto 1fr auto;
  gap: 10px;
  height: 100%;
}

.job-card.paid {
  border-left: 8px solid #27ae60;
}

.job-card.unpaid {
  border-left: 8px solid #c0392b;
}

.job-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
}

/* ---------------- HEADER ---------------- */
.job-header {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 15px;
}

.company-logo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.company-name {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2c3e50;
}

.job-title {
  font-size: 1.3rem;
  color: #34495e;
}

.job-type {
  font-size: 1rem;
  color: #7f8c8d;
}

/* ---------------- CONTENT ---------------- */
.salary {
  font-size: 1.1rem;
  font-weight: bold;
  color: #16a085;
}

.description {
  font-size: 1rem;
  color: #555;
  line-height: 1.6;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* ---------------- FOOTER ---------------- */
.card-footer {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-top: auto;
}

.apply-button {
  background-color: #3498db;
  color: #fff;
  padding: 10px 18px;
  border-radius: 6px;
  text-decoration: none;
  font-weight: bold;
}

.bookmark-btn {
  background-color: #f1c40f;
  border: none;
  padding: 8px 14px;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}

.bookmark-btn.bookmarked {
  background-color: #f39c12;
  color: white;
}

.bookmark-btn:hover {
  opacity: 0.9;
}

.remote-tag {
  margin-left: auto;
  background-color: #f39c12;
  color: white;
  padding: 6px 12px;
  border-radius: 4px;
  font-weight: bold;
}
</style>
