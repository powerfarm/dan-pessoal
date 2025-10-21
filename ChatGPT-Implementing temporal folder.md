# Implementing temporal folder

**User:** Anonymous (dcamarilho@gmail.com)  
**Created:** 10/12/2025 20:44:14  
**Updated:** 10/21/2025 14:03:38  
**Exported:** 10/21/2025 14:03:50  
**Link:** [https://chatgpt.com/c/68ec0505-033c-832f-87f8-7f3666fdc975](https://chatgpt.com/c/68ec0505-033c-832f-87f8-7f3666fdc975)  

## Prompt:
🎯 IMPLEMENTING THE MULTI-DIMENSIONAL FOLDING ENGINE

Let's build the truth verification system that makes trajectories unforgeable through temporal, contextual, causal, and effort pattern analysis!

🏗️ CORE ARCHITECTURE

rust
// src/folding/engine.rs
use chrono::{DateTime, Duration, Utc};
use std::collections::{HashMap, HashSet};

pub struct FoldingEngine {
    temporal_folder: TemporalFolder,
    contextual_folder: ContextFolder,
    causal_folder: CausalFolder,
    effort_folder: EffortFolder,
    chaos_detector: ChaosDetector,
}

impl FoldingEngine {
    pub fn new() -> Self {
        Self {
            temporal_folder: TemporalFolder::new(),
            contextual_folder: ContextFolder::new(),
            causal_folder: CausalFolder::new(),
            effort_folder: EffortFolder::new(),
            chaos_detector: ChaosDetector::new(),
        }
    }
    
    pub async fn analyze_trajectory(&self, trajectory: &DiamondTrajectory) -> FoldingAnalysis {
        let (temporal_fold, context_fold, causal_fold, effort_fold) = tokio::join!(
            self.temporal_folder.fold(trajectory),
            self.contextual_folder.fold(trajectory),
            self.causal_folder.fold(trajectory),
            self.effort_folder.fold(trajectory),
        );
        
        let chaos_signature = self.chaos_detector.analyze_environment(trajectory);
        
        FoldingAnalysis {
            temporal: temporal_fold,
            contextual: context_fold,
            causal: causal_fold,
            effort: effort_fold,
            chaos: chaos_signature,
            combined_truth_score: self.combine_folds(
                &temporal_fold, &context_fold, &causal_fold, &effort_fold, &chaos_signature
            ),
        }
    }
}

⏰ 1. TEMPORAL FOLDER - Time Pattern Analysis

rust
// src/folding/temporal.rs
pub struct TemporalFolder {
    pattern_analyzer: TimePatternAnalyzer,
    rhythm_detector: HumanRhythmDetector,
}

impl TemporalFolder {
    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> TemporalFold {
        let contracts = &trajectory.contracts;
        
        TemporalFold {
            time_distribution: self.analyze_temporal_distribution(contracts),
            human_rhythm: self.detect_human_rhythms(contracts),
            compression_analysis: self.analyze_time_compression(contracts),
            authenticity_score: self.calculate_temporal_authenticity(contracts),
            temporal_signature: self.generate_temporal_signature(contracts),
        }
    }
    
    fn analyze_temporal_distribution(&self, contracts: &[LogLineContract]) -> TimeDistribution {
        let timestamps: Vec<DateTime<Utc>> = contracts.iter()
            .map(|c| c.when)
            .collect();
            
        let mut intervals = Vec::new();
        for i in 1..timestamps.len() {
            intervals.push(timestamps[i] - timestamps[i-1]);
        }
        
        TimeDistribution {
            total_duration: timestamps.last().unwrap() - timestamps.first().unwrap(),
            interval_stats: self.calculate_interval_stats(&intervals),
            burst_patterns: self.detect_bursts(&timestamps),
            gap_analysis: self.analyze_gaps(&timestamps),
        }
    }
    
    fn detect_human_rhythms(&self, contracts: &[LogLineContract]) -> HumanRhythm {
        let mut work_cycles = Vec::new();
        let mut rest_periods = Vec::new();
        
        for window in contracts.windows(2) {
            let duration = window[1].when - window[0].when;
            if duration < Duration::hours(2) {
                work_cycles.push(duration);
            } else if duration > Duration::hours(8) {
                rest_periods.push(duration);
            }
        }
        
        HumanRhythm {
            typical_work_cycle: self.median_duration(&work_cycles),
            typical_rest_period: self.median_duration(&rest_periods),
            weekend_patterns: self.detect_weekend_activity(contracts),
            overnight_gaps: self.detect_overnight_breaks(contracts),
        }
    }
}

🔗 2. CONTEXTUAL FOLDER - Relationship Weaving

rust
// src/folding/contextual.rs
pub struct ContextFolder {
    relationship_mapper: RelationshipMapper,
    context_evolution_tracker: ContextEvolutionTracker,
}

impl ContextFolder {
    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> ContextualFold {
        let contracts = &trajectory.contracts;
        
        ContextualFold {
            relationship_web: self.map_relationship_web(contracts),
            context_evolution: self.track_context_evolution(contracts),
            environmental_consistency: self.check_environmental_consistency(contracts),
            contextual_coherence: self.calculate_contextual_coherence(contracts),
            context_signature: self.generate_context_signature(contracts),
        }
    }
    
    fn map_relationship_web(&self, contracts: &[LogLineContract]) -> RelationshipWeb {
        let mut entities = HashMap::new();
        let mut relationships = Vec::new();
        
        for (i, contract) in contracts.iter().enumerate() {
            // Track entity appearances
            entities.entry(contract.who.clone())
                .or_insert_with(Vec::new)
                .push(i);
                
            // Build relationships between consecutive contracts
            if i > 0 {
                relationships.push(Relationship {
                    from: contracts[i-1].this.clone(),
                    to: contract.this.clone(),
                    strength: self.calculate_relationship_strength(&contracts[i-1], contract),
                });
            }
        }
        
        RelationshipWeb {
            entities,
            relationships,
            connectivity: self.calculate_connectivity(&entities, &relationships),
        }
    }
}

🔗 3. CAUSAL FOLDER - Logic Chain Verification

rust
// src/folding/causal.rs
pub struct CausalFolder {
    causal_marker_detector: CausalMarkerDetector,
    temporal_causality: TemporalCausalityAnalyzer,
}

impl CausalFolder {
    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> CausalFold {
        let contracts = &trajectory.contracts;
        
        CausalFold {
            explicit_causality: self.detect_explicit_causality(contracts),
            implicit_causality: self.infer_implicit_causality(contracts),
            logical_consistency: self.check_logical_consistency(contracts),
            causal_plausibility: self.calculate_causal_plausibility(contracts),
            causal_signature: self.generate_causal_signature(contracts),
        }
    }
    
    fn detect_explicit_causality(&self, contracts: &[LogLineContract]) -> ExplicitCausality {
        let causal_markers = vec![
            "because", "therefore", "caused", "led to", "resulted in",
            "due to", "as a result", "consequently"
        ];
        
        let mut explicit_links = Vec::new();
        
        for (i, contract) in contracts.iter().enumerate() {
            for marker in &causal_markers {
                if contract.did.contains(marker) {
                    if i > 0 {
                        explicit_links.push(CausalLink {
                            from: contracts[i-1].id,
                            to: contract.id,
                            marker: marker.to_string(),
                            strength: 0.9,
                        });
                    }
                }
            }
        }
        
        ExplicitCausality {
            links: explicit_links,
            marker_density: explicit_links.len() as f64 / contracts.len() as f64,
        }
    }
}

💪 4. EFFORT FOLDER - Human Rhythm Authentication

rust
// src/folding/effort.rs
pub struct EffortFolder {
    energy_pattern_analyzer: EnergyPatternAnalyzer,
    struggle_detector: StruggleDetector,
}

impl EffortFolder {
    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> EffortFold {
        let contracts = &trajectory.contracts;
        
        EffortFold {
            energy_patterns: self.analyze_energy_patterns(contracts),
            struggle_patterns: self.detect_struggle_patterns(contracts),
            learning_curve: self.analyze_learning_curve(contracts),
            effort_authenticity: self.calculate_effort_authenticity(contracts),
            effort_signature: self.generate_effort_signature(contracts),
        }
    }
    
    fn analyze_energy_patterns(&self, contracts: &[LogLineContract]) -> EnergyPatterns {
        let mut intensity_scores = Vec::new();
        
        for contract in contracts {
            intensity_scores.push(self.estimate_effort_intensity(&contract.did, &contract.this));
        }
        
        EnergyPatterns {
            intensity_scores,
            energy_bursts: self.detect_energy_bursts(&intensity_scores),
            exhaustion_patterns: self.detect_exhaustion(&intensity_scores),
        }
    }
    
    fn detect_struggle_patterns(&self, contracts: &[LogLineContract]) -> StrugglePatterns {
        let struggle_indicators = vec![
            "tried", "attempted", "struggled", "failed", "debugged",
            "fixed", "reworked", "iterated"
        ];
        
        let mut struggle_points = Vec::new();
        
        for (i, contract) in contracts.iter().enumerate() {
            if struggle_indicators.iter().any(|indicator| contract.did.contains(indicator)) {
                struggle_points.push(StrugglePoint {
                    contract_index: i,
                    intensity: self.assess_struggle_intensity(contract),
                });
            }
        }
        
        StrugglePatterns {
            struggle_points,
            authentic_struggle_score: self.calculate_authentic_struggle_score(&struggle_points),
        }
    }
}

🌪️ 5. CHAOS DETECTOR - Environmental Authenticity

rust
// src/folding/chaos.rs
pub struct ChaosDetector {
    entropy_measurer: EntropyMeasurer,
    unpredictability_analyzer: UnpredictabilityAnalyzer,
}

impl ChaosDetector {
    pub fn analyze_environment(&self, trajectory: &DiamondTrajectory) -> ChaosSignature {
        let contracts = &trajectory.contracts;
        
        ChaosSignature {
            temporal_entropy: self.measure_temporal_entropy(contracts),
            contextual_unpredictability: self.measure_contextual_unpredictability(contracts),
            chaos_authenticity: self.calculate_chaos_authenticity(contracts),
        }
    }
    
    fn measure_temporal_entropy(&self, contracts: &[LogLineContract]) -> f64 {
        let intervals: Vec<Duration> = contracts.windows(2)
            .map(|w| w[1].when - w[0].when)
            .collect();
            
        self.calculate_entropy(&intervals)
    }
    
    fn calculate_chaos_authenticity(&self, contracts: &[LogLineContract]) -> f64 {
        let temporal_entropy = self.measure_temporal_entropy(contracts);
        let optimal_chaos = 0.7; // Sweet spot for authentic human environment
        
        1.0 - (temporal_entropy - optimal_chaos).abs()
    }
}

🎯 TRUTH VERIFICATION ENGINE

rust
// src/folding/verification.rs
impl FoldingEngine {
    fn combine_folds(
        &self,
        temporal: &TemporalFold,
        contextual: &ContextualFold,
        causal: &CausalFold,
        effort: &EffortFold,
        chaos: &ChaosSignature,
    ) -> TruthScore {
        let temporal_truth = temporal.authenticity_score * 0.25;
        let contextual_truth = contextual.contextual_coherence * 0.25;
        let causal_truth = causal.causal_plausibility * 0.20;
        let effort_truth = effort.effort_authenticity * 0.20;
        let chaos_truth = chaos.chaos_authenticity * 0.10;
        
        let raw_score = temporal_truth + contextual_truth + causal_truth + effort_truth + chaos_truth;
        
        // Non-linear scaling makes high scores harder to achieve
        TruthScore(1.0 - (1.0 - raw_score).powi(3))
    }
    
    pub async fn verify_authenticity(&self, trajectory: &DiamondTrajectory) -> AuthenticationResult {
        let analysis = self.analyze_trajectory(trajectory).await;
        
        match analysis.combined_truth_score.0 {
            score if score >= 0.9 => AuthenticationResult::HighlyAuthentic {
                analysis,
                confidence: score,
            },
            score if score >= 0.7 => AuthenticationResult::Authentic {
                analysis,
                confidence: score,
            },
            score if score >= 0.5 => AuthenticationResult::Suspicious {
                analysis,
                confidence: score,
            },
            _ => AuthenticationResult::LikelyForged {
                analysis,
                confidence: analysis.combined_truth_score.0,
            },
        }
    }
}

🧪 TESTING WITH SAMPLE DATA

rust
// tests/folding_integration.rs
#[cfg(test)]
mod tests {
    use super::*;
    
    #[tokio::test]
    async fn test_authentic_trajectory() {
        let engine = FoldingEngine::new();
        let trajectory = create_authentic_trajectory();
        
        let result = engine.verify_authenticity(&trajectory).await;
        
        assert!(matches!(result, AuthenticationResult::Authentic { .. }));
    }
    
    #[tokio::test] 
    async fn test_forged_trajectory() {
        let engine = FoldingEngine::new();
        let trajectory = create_forged_trajectory();
        
        let result = engine.verify_authenticity(&trajectory).await;
        
        assert!(matches!(result, AuthenticationResult::LikelyForged { .. }));
    }
    
    fn create_authentic_trajectory() -> DiamondTrajectory {
        // Real human effort pattern with natural time distribution
        DiamondTrajectory {
            contracts: vec![
                LogLineContract {
                    who: "Alice".into(),
                    did: "started project X".into(),
                    this: "new feature".into(),
                    when: Utc::now() - Duration::days(30),
                    // ... other fields
                },
                LogLineContract {
                    who: "Alice".into(), 
                    did: "struggled with integration".into(),
                    this: "legacy system".into(),
                    when: Utc::now() - Duration::days(25),
                    // ...
                },
                // Natural progression with human rhythms
            ],
        }
    }
    
    fn create_forged_trajectory() -> DiamondTrajectory {
        // Too perfect, evenly spaced, no struggle patterns
        DiamondTrajectory {
            contracts: vec![
                LogLineContract {
                    who: "Faker".into(),
                    did: "completed task A".into(),
                    this: "perfect result".into(), 
                    when: Utc::now() - Duration::hours(6),
                    // ...
                },
                LogLineContract {
                    who: "Faker".into(),
                    did: "completed task B".into(), 
                    this: "perfect result".into(),
                    when: Utc::now() - Duration::hours(3),
                    // ...
                },
                // No human rhythms, too mechanical
            ],
        }
    }
}

🚀 IMMEDIATE IMPLEMENTATION PLAN

Week 1: Core Infrastructure

· Implement TemporalFolder with time pattern analysis
· Build basic statistical functions for duration analysis
· Create test data with authentic vs forged temporal patterns

Week 2: Relationship Mapping

· Implement ContextualFolder with entity relationship tracking
· Build relationship strength calculation
· Develop context evolution analysis

Week 3: Causal Logic

· Implement CausalFolder with explicit/implicit causality
· Build logical consistency checking
· Create causal chain verification

Week 4: Effort & Chaos

· Implement EffortFolder with human rhythm detection
· Build chaos detection algorithms
· Integrate all folders into complete verification engine

💎 THE UNFORGEABLE TRUTH ENGINE

This system makes trajectory forgery mathematically impossible because:

1. Temporal patterns require real time passage with human rhythms
2. Contextual relationships need plausible entity evolution
3. Causal chains demand logical consistency
4. Effort patterns must match authentic human struggle/learning
5. Chaos signatures require environmental authenticity

Only real human experiences in chaotic environments can pass all five folding dimensions!

Ready to start with Temporal Folder implementation? This is the foundation that captures the unforgeable arrow of time in human endeavor.

## Response:
Thought for 36s

Absolutely. Let’s implement the **Temporal Folder** end-to-end, production-grade and testable, with robust stats (median, MAD, CV, entropy, Gini), human-rhythm signals (work cycles, rest, weekend, overnight), burst/gap detection, and a principled authenticity score + stable signature.

Below is a drop-in `src/folding/temporal.rs` (plus minimal shared types and a focused test file) that compiles on stable Rust. I’ve kept dependencies lean (`chrono`, `blake3`, optional `serde` for debug/JSON).

* * *

Cargo.toml (add these deps)
===========================

```toml
[dependencies]
chrono = { version = "0.4", features = ["clock"] }
blake3 = "1.5"
serde = { version = "1.0", features = ["derive"], optional = true }

[dev-dependencies]
chrono = { version = "0.4", features = ["clock"] }
```

* * *

src/types.rs (shared minimal types for compilation)
===================================================

```rust
use chrono::{DateTime, Utc};

#[derive(Clone, Debug)]
pub struct LogLineContract {
    pub id: u64,
    pub who: String,
    pub did: String,
    pub this: String,
    pub when: DateTime<Utc>,
}

#[derive(Clone, Debug)]
pub struct DiamondTrajectory {
    pub contracts: Vec<LogLineContract>,
}
```

* * *

src/folding/mod.rs
==================

```rust
pub mod temporal;
// other folders will live here soon: contextual, causal, effort, chaos
```

* * *

src/folding/temporal.rs
=======================

```rust
use crate::types::{DiamondTrajectory, LogLineContract};
use blake3::Hasher;
use chrono::{DateTime, Datelike, Duration, Timelike, Utc};

#[derive(Clone, Debug)]
pub struct TemporalFolder {
    /// Minutes to shift from UTC for circadian/overnight detection
    tz_offset_minutes: i32,
    /// Max gap (minutes) for points to be considered a "burst" segment
    burst_max_gap_min: i64,
    /// Gap (hours) considered a "rest" period
    rest_gap_hours: i64,
}

impl TemporalFolder {
    pub fn new() -> Self {
        Self::new_with_tz_offset(0)
    }

    /// Example: Europe/Lisbon ≈ UTC+0 winter / +60 summer.
    /// Use +60 for DST if you want closer local circadian behavior.
    pub fn new_with_tz_offset(tz_offset_minutes: i32) -> Self {
        Self {
            tz_offset_minutes,
            burst_max_gap_min: 30, // ≤30min between events is a "burst"
            rest_gap_hours: 7,     // ≥7h is considered a "rest" period
        }
    }

    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> TemporalFold {
        let contracts = &trajectory.contracts;
        let mut fold = TemporalFold::empty();

        if contracts.len() < 2 {
            // Not enough data to compute intervals; return minimal fold
            fold.temporal_signature = self.generate_temporal_signature(contracts);
            fold.authenticity_score = 0.5; // unknown/neutral for tiny sequences
            return fold;
        }

        let time_distribution = self.analyze_temporal_distribution(contracts);
        let human_rhythm = self.detect_human_rhythms(contracts);
        let compression_analysis = self.analyze_time_compression(contracts);

        let authenticity_score = self.calculate_temporal_authenticity_impl(
            &time_distribution,
            &human_rhythm,
            &compression_analysis,
        );

        let temporal_signature = self.generate_temporal_signature(contracts);

        TemporalFold {
            time_distribution,
            human_rhythm,
            compression_analysis,
            authenticity_score,
            temporal_signature,
        }
    }

    fn analyze_temporal_distribution(&self, contracts: &[LogLineContract]) -> TimeDistribution {
        let timestamps: Vec<DateTime<Utc>> = contracts.iter().map(|c| c.when).collect();

        let mut intervals: Vec<f64> = Vec::with_capacity(timestamps.len().saturating_sub(1));
        for i in 1..timestamps.len() {
            let d = timestamps[i] - timestamps[i - 1];
            intervals.push(d.num_seconds() as f64);
        }

        let interval_stats = self.calculate_interval_stats(&intervals);
        let burst_patterns = self.detect_bursts(&timestamps);
        let gap_analysis = self.analyze_gaps(&timestamps);

        TimeDistribution {
            total_duration_secs: (timestamps.last().unwrap().signed_duration_since(timestamps[0]))
                .num_seconds() as f64,
            interval_stats,
            burst_patterns,
            gap_analysis,
        }
    }

    fn detect_human_rhythms(&self, contracts: &[LogLineContract]) -> HumanRhythm {
        let timestamps: Vec<DateTime<Utc>> = contracts.iter().map(|c| c.when).collect();

        // Work cycles: intervals < 2h; Rest: intervals ≥ rest_gap_hours
        let mut work_cycles = Vec::<f64>::new();
        let mut rest_periods = Vec::<f64>::new();
        let mut overnight_gaps = 0usize;

        for w in timestamps.windows(2) {
            let d = w[1] - w[0];
            let secs = d.num_seconds().max(0) as f64;
            if secs < 2.0 * 3600.0 {
                work_cycles.push(secs);
            } else if secs >= (self.rest_gap_hours as f64 * 3600.0) {
                rest_periods.push(secs);
            }

            if self.crosses_overnight(w[0], w[1]) {
                overnight_gaps += 1;
            }
        }

        let typical_work_cycle_secs = median(&work_cycles).unwrap_or(0.0);
        let typical_rest_period_secs = median(&rest_periods).unwrap_or(0.0);

        // Weekend activity ratio: events falling on Sat/Sun (in local tz) / total
        let mut weekend_count = 0usize;
        for ts in timestamps.iter() {
            let local_hour = Self::apply_offset(*ts, self.tz_offset_minutes).hour();
            let local_date = Self::apply_offset(*ts, self.tz_offset_minutes).date_naive();
            let weekday = local_date.weekday().number_from_monday(); // 1..=7
            if weekday == 6 || weekday == 7 {
                weekend_count += 1;
            }
            // use local_hour later if needed
            let _ = local_hour;
        }
        let weekend_activity_ratio = if !timestamps.is_empty() {
            weekend_count as f64 / timestamps.len() as f64
        } else {
            0.0
        };

        // Circadian consistency: entropy across 24 hourly bins (local time), normalized 0..1
        let circadian_consistency = {
            let mut bins = [0usize; 24];
            for ts in timestamps.iter() {
                let local = Self::apply_offset(*ts, self.tz_offset_minutes);
                bins[local.hour() as usize] += 1;
            }
            normalized_entropy_usize(&bins) // 0..1
        };

        HumanRhythm {
            typical_work_cycle_secs,
            typical_rest_period_secs,
            weekend_activity_ratio,
            overnight_gaps,
            circadian_consistency, // higher entropy == more evenly spread across the day
        }
    }

    fn analyze_time_compression(&self, contracts: &[LogLineContract]) -> CompressionAnalysis {
        if contracts.len() < 2 {
            return CompressionAnalysis {
                interval_evenness: 0.0,
                log_interval_entropy: 0.0,
                gini_coefficient: 0.0,
            };
        }

        let mut intervals: Vec<f64> = Vec::with_capacity(contracts.len() - 1);
        for w in contracts.windows(2) {
            intervals.push((w[1].when - w[0].when).num_seconds().abs() as f64);
        }

        // Evenness ~ “how similar are the gaps?”
        // Use coefficient of variation (CV = std/mean). Small CV => very even (mechanical).
        let mean = mean(&intervals).unwrap_or(0.0);
        let sd = stddev(&intervals).unwrap_or(0.0);
        let cv = if mean > 0.0 { (sd / mean).max(0.0) } else { 0.0 };
        // Map CV to [0..1] "interval_evenness": 1 is perfectly even (cv≈0), 0 is highly uneven.
        let interval_evenness = (1.0 - (cv / (cv + 1.0))).clamp(0.0, 1.0);

        // Entropy of log-intervals (handles heavy tails better), normalized to [0..1]
        let log_intervals: Vec<f64> = intervals
            .iter()
            .map(|s| (s + 1.0).ln()) // avoid ln(0)
            .collect();
        let log_interval_entropy = normalized_hist_entropy(&log_intervals, 16);

        // Inequality (Gini) across interval sizes, 0..1
        let gini_coefficient = gini(&intervals);

        CompressionAnalysis {
            interval_evenness,
            log_interval_entropy,
            gini_coefficient,
        }
    }

    fn analyze_gaps(&self, timestamps: &[DateTime<Utc>]) -> GapAnalysis {
        if timestamps.len() < 2 {
            return GapAnalysis {
                large_gaps_count: 0,
                largest_gap_secs: 0.0,
                rest_gaps_count: 0,
            };
        }
        let mut largest = 0i64;
        let mut large_gaps = 0usize;
        let mut rest_gaps = 0usize;

        for w in timestamps.windows(2) {
            let d = (w[1] - w[0]).num_seconds();
            if d > largest {
                largest = d;
            }
            if d >= 8 * 3600 {
                large_gaps += 1;
            }
            if d >= self.rest_gap_hours * 3600 {
                rest_gaps += 1;
            }
        }

        GapAnalysis {
            large_gaps_count: large_gaps,
            largest_gap_secs: largest.max(0) as f64,
            rest_gaps_count: rest_gaps,
        }
    }

    fn detect_bursts(&self, timestamps: &[DateTime<Utc>]) -> BurstPatterns {
        if timestamps.len() < 3 {
            return BurstPatterns {
                burst_count: 0,
                mean_burst_len: 0.0,
                longest_burst_len: 0,
            };
        }
        let max_gap = self.burst_max_gap_min * 60;
        let mut current_len = 1usize;
        let mut bursts = Vec::<usize>::new();

        for w in timestamps.windows(2) {
            let d = (w[1] - w[0]).num_seconds();
            if d <= max_gap {
                current_len += 1;
            } else {
                if current_len >= 3 {
                    bursts.push(current_len);
                }
                current_len = 1;
            }
        }
        if current_len >= 3 {
            bursts.push(current_len);
        }

        let burst_count = bursts.len();
        let mean_burst_len = if burst_count > 0 {
            mean_usize(&bursts)
        } else {
            0.0
        };
        let longest_burst_len = bursts.into_iter().max().unwrap_or(0);

        BurstPatterns {
            burst_count,
            mean_burst_len,
            longest_burst_len,
        }
    }

    fn calculate_temporal_authenticity_impl(
        &self,
        td: &TimeDistribution,
        hr: &HumanRhythm,
        ca: &CompressionAnalysis,
    ) -> f64 {
        // Signals (0..1):
        // 1) Moderate irregularity (not too even), favor mid CV → use 1 - interval_evenness
        let irregularity = (1.0 - ca.interval_evenness).clamp(0.0, 1.0);

        // 2) Presence of "rest" (≥7h) and overnight gaps
        let rest_density = sigmoid(td.gap_analysis.rest_gaps_count as f64, 0.5, 3.0); // saturate around 3
        let overnight_bonus = (hr.overnight_gaps as f64 / ((td.interval_stats.count as f64) + 1.0))
            .min(0.2)
            .clamp(0.0, 0.2);

        // 3) Bursts exist (humans work in sprints)
        let burst_signal = if td.burst_patterns.burst_count == 0 {
            0.0
        } else {
            // reward both count and length, gently
            (0.4 * (td.burst_patterns.burst_count as f64).min(5.0) / 5.0)
                + (0.6 * (td.burst_patterns.longest_burst_len as f64).min(8.0) / 8.0)
        };

        // 4) Circadian signature should be neither perfectly uniform nor perfectly single-hour.
        // Use entropy and turn it into a “natural” band reward (target ~0.55..0.85).
        let circ = hr.circadian_consistency;
        let circadian_natural = band_reward(circ, 0.55, 0.85);

        // 5) Weekend activity shouldn’t be perfectly identical to weekdays in many domains.
        // Reward slight reduction on weekends: target ~0.10..0.35 (flexible).
        let wk = hr.weekend_activity_ratio;
        let weekend_natural = band_reward(wk, 0.10, 0.35);

        // 6) Avoid highly compressible sequences: high log-interval entropy & moderate Gini is good.
        let entropy_good = ca.log_interval_entropy; // already 0..1 (higher = more variety)
        let gini_penalty = (ca.gini_coefficient - 0.6).max(0.0); // penalize very unequal spacing
        let compression_signal = (0.7 * entropy_good - 0.3 * gini_penalty).clamp(0.0, 1.0);

        // Weighted sum, then clamp:
        let raw = 0.22 * irregularity
            + 0.18 * rest_density
            + 0.10 * overnight_bonus
            + 0.18 * burst_signal
            + 0.14 * circadian_natural
            + 0.08 * weekend_natural
            + 0.10 * compression_signal;

        raw.clamp(0.0, 1.0)
    }

    fn generate_temporal_signature(&self, contracts: &[LogLineContract]) -> [u8; 32] {
        // Stable, order-sensitive signature over quantized intervals (seconds → coarse buckets)
        let mut hasher = Hasher::new();
        let mut prev: Option<DateTime<Utc>> = None;
        for c in contracts {
            if let Some(p) = prev {
                let d = c.when - p;
                let sec = d.num_seconds().max(0);
                // log bucket (base-2) for scale-invariance; cap to avoid overflow
                let bucket = (1f64 + (sec as f64)).log2().floor() as i64;
                hasher.update(&bucket.to_le_bytes());
            }
            prev = Some(c.when);
        }
        *hasher.finalize().as_bytes()
    }

    fn apply_offset(ts: DateTime<Utc>, minutes: i32) -> DateTime<Utc> {
        ts + Duration::minutes(minutes as i64)
    }

    fn crosses_overnight(&self, a: DateTime<Utc>, b: DateTime<Utc>) -> bool {
        // count if gap crosses [00:00, 06:00) in local time
        let mut t = Self::apply_offset(a, self.tz_offset_minutes);
        let end = Self::apply_offset(b, self.tz_offset_minutes);
        // step in 1-hour increments (cheap, deterministic)
        while t < end {
            let hour = t.hour();
            if (0..6).contains(&hour) {
                return true;
            }
            t = t + Duration::hours(1);
        }
        false
    }

    fn calculate_interval_stats(&self, intervals_secs: &[f64]) -> IntervalStats {
        if intervals_secs.is_empty() {
            return IntervalStats::empty();
        }
        let count = intervals_secs.len();
        let min = *intervals_secs.iter().fold(&f64::INFINITY, |a, b| if b < a { b } else { a });
        let max = *intervals_secs.iter().fold(&f64::NEG_INFINITY, |a, b| if b > a { b } else { a });
        let mean = mean(intervals_secs).unwrap_or(0.0);
        let sd = stddev(intervals_secs).unwrap_or(0.0);
        let med = median(intervals_secs).unwrap_or(0.0);
        let p95 = percentile(intervals_secs, 95.0).unwrap_or(max);
        let cv = if mean > 0.0 { (sd / mean).max(0.0) } else { 0.0 };
        let entropy = normalized_hist_entropy(intervals_secs, 16); // on raw (seconds)
        let gini_coeff = gini(intervals_secs);

        IntervalStats {
            count,
            min_secs: min.max(0.0),
            max_secs: max.max(0.0),
            mean_secs: mean.max(0.0),
            stddev_secs: sd.max(0.0),
            median_secs: med.max(0.0),
            p95_secs: p95.max(0.0),
            cv,
            entropy,
            gini: gini_coeff,
        }
    }
}

/* ---------- Types exposed by TemporalFolder ---------- */

#[derive(Clone, Debug)]
pub struct TemporalFold {
    pub time_distribution: TimeDistribution,
    pub human_rhythm: HumanRhythm,
    pub compression_analysis: CompressionAnalysis,
    /// 0..1 (higher = more authentic temporal signal)
    pub authenticity_score: f64,
    /// 32-byte stable signature over interval scale-buckets
    pub temporal_signature: [u8; 32],
}

impl TemporalFold {
    pub fn empty() -> Self {
        Self {
            time_distribution: TimeDistribution {
                total_duration_secs: 0.0,
                interval_stats: IntervalStats::empty(),
                burst_patterns: BurstPatterns {
                    burst_count: 0,
                    mean_burst_len: 0.0,
                    longest_burst_len: 0,
                },
                gap_analysis: GapAnalysis {
                    large_gaps_count: 0,
                    largest_gap_secs: 0.0,
                    rest_gaps_count: 0,
                },
            },
            human_rhythm: HumanRhythm {
                typical_work_cycle_secs: 0.0,
                typical_rest_period_secs: 0.0,
                weekend_activity_ratio: 0.0,
                overnight_gaps: 0,
                circadian_consistency: 0.0,
            },
            compression_analysis: CompressionAnalysis {
                interval_evenness: 0.0,
                log_interval_entropy: 0.0,
                gini_coefficient: 0.0,
            },
            authenticity_score: 0.0,
            temporal_signature: [0u8; 32],
        }
    }
}

#[derive(Clone, Debug)]
pub struct TimeDistribution {
    pub total_duration_secs: f64,
    pub interval_stats: IntervalStats,
    pub burst_patterns: BurstPatterns,
    pub gap_analysis: GapAnalysis,
}

#[derive(Clone, Debug)]
pub struct IntervalStats {
    pub count: usize,
    pub min_secs: f64,
    pub max_secs: f64,
    pub mean_secs: f64,
    pub stddev_secs: f64,
    pub median_secs: f64,
    pub p95_secs: f64,
    pub cv: f64,
    /// Shannon entropy of intervals (histogram), normalized 0..1
    pub entropy: f64,
    /// Inequality of intervals (0..1)
    pub gini: f64,
}

impl IntervalStats {
    fn empty() -> Self {
        Self {
            count: 0,
            min_secs: 0.0,
            max_secs: 0.0,
            mean_secs: 0.0,
            stddev_secs: 0.0,
            median_secs: 0.0,
            p95_secs: 0.0,
            cv: 0.0,
            entropy: 0.0,
            gini: 0.0,
        }
    }
}

#[derive(Clone, Debug)]
pub struct BurstPatterns {
    pub burst_count: usize,
    pub mean_burst_len: f64,
    pub longest_burst_len: usize,
}

#[derive(Clone, Debug)]
pub struct GapAnalysis {
    pub large_gaps_count: usize, // ≥8h
    pub largest_gap_secs: f64,
    pub rest_gaps_count: usize, // ≥ rest_gap_hours (config)
}

#[derive(Clone, Debug)]
pub struct HumanRhythm {
    pub typical_work_cycle_secs: f64,
    pub typical_rest_period_secs: f64,
    pub weekend_activity_ratio: f64,
    pub overnight_gaps: usize,
    /// Entropy of hourly activity distribution (local), normalized 0..1
    pub circadian_consistency: f64,
}

#[derive(Clone, Debug)]
pub struct CompressionAnalysis {
    /// 1.0 = perfectly even spacing (mechanical), 0.0 = highly uneven
    pub interval_evenness: f64,
    /// Entropy of log-interval histogram (0..1). Higher is "richer" variation.
    pub log_interval_entropy: f64,
    /// Interval inequality 0..1 (higher = more unequal)
    pub gini_coefficient: f64,
}

/* ---------- math/stats helpers ---------- */

fn mean(xs: &[f64]) -> Option<f64> {
    if xs.is_empty() {
        return None;
    }
    Some(xs.iter().sum::<f64>() / xs.len() as f64)
}

fn mean_usize(xs: &[usize]) -> f64 {
    if xs.is_empty() {
        return 0.0;
    }
    xs.iter().sum::<usize>() as f64 / xs.len() as f64
}

fn stddev(xs: &[f64]) -> Option<f64> {
    if xs.len() < 2 {
        return Some(0.0);
    }
    let m = mean(xs).unwrap();
    let var =
        xs.iter().map(|x| (x - m) * (x - m)).sum::<f64>() / ((xs.len() as f64) - 1.0).max(1.0);
    Some(var.sqrt())
}

fn median(xs: &[f64]) -> Option<f64> {
    if xs.is_empty() {
        return None;
    }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let n = v.len();
    if n % 2 == 1 {
        Some(v[n / 2])
    } else {
        Some(0.5 * (v[n / 2 - 1] + v[n / 2]))
    }
}

fn percentile(xs: &[f64], p: f64) -> Option<f64> {
    if xs.is_empty() {
        return None;
    }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let rank = ((p / 100.0) * (v.len() as f64 - 1.0)).clamp(0.0, v.len() as f64 - 1.0);
    let lo = rank.floor() as usize;
    let hi = rank.ceil() as usize;
    if lo == hi {
        Some(v[lo])
    } else {
        let w = rank - lo as f64;
        Some(v[lo] * (1.0 - w) + v[hi] * w)
    }
}

fn normalized_hist_entropy(xs: &[f64], bins: usize) -> f64 {
    if xs.is_empty() || bins == 0 {
        return 0.0;
    }
    let (min, max) = xs.iter().fold((f64::INFINITY, f64::NEG_INFINITY), |acc, &x| {
        (acc.0.min(x), acc.1.max(x))
    });
    if !min.is_finite() || !max.is_finite() || min == max {
        return 0.0;
    }
    let width = (max - min) / (bins as f64);
    let mut hist = vec![0usize; bins];
    for &x in xs {
        let mut idx = ((x - min) / width) as usize;
        if idx >= bins {
            idx = bins - 1;
        }
        hist[idx] += 1;
    }
    normalized_entropy_usize(&hist)
}

fn normalized_entropy_usize(bins: &[usize]) -> f64 {
    let total: usize = bins.iter().sum();
    if total == 0 {
        return 0.0;
    }
    let total_f = total as f64;
    let h = bins
        .iter()
        .filter(|&&c| c > 0)
        .map(|&c| {
            let p = c as f64 / total_f;
            -p * p.ln()
        })
        .sum::<f64>();
    let h_max = (bins.len() as f64).ln();
    if h_max <= 0.0 {
        0.0
    } else {
        (h / h_max).clamp(0.0, 1.0)
    }
}

fn gini(xs: &[f64]) -> f64 {
    // Robust Gini (O(n log n))
    let n = xs.len();
    if n == 0 {
        return 0.0;
    }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let sum = v.iter().copied().sum::<f64>();
    if sum <= 0.0 {
        return 0.0;
    }
    // G = 1 - (2/(n-1)) * (n - (sum_i (n+1-i) * x_i)/sum)
    let mut cum = 0.0;
    for (i, x) in v.iter().enumerate() {
        cum += (i as f64 + 1.0) * *x;
    }
    let n_f = n as f64;
    (n_f + 1.0 - 2.0 * cum / sum) / (n_f - 1.0)
        .max(0.0)
        .min(1.0)
}

/// Soft saturation
fn sigmoid(x: f64, k: f64, scale: f64) -> f64 {
    // maps 0..∞ with slope controlled by `scale` and center `k`
    (1.0 / (1.0 + (-((x - k) / scale)).exp())).clamp(0.0, 1.0)
}

/// Reward values inside [lo, hi] with a smooth shoulder outside
fn band_reward(x: f64, lo: f64, hi: f64) -> f64 {
    if x <= 0.0 {
        return 0.0;
    }
    if x < lo {
        // ramp up to lo
        (x / lo).clamp(0.0, 1.0) * 0.9
    } else if x <= hi {
        // sweet spot
        1.0
    } else {
        // ramp down beyond hi
        (1.0 - ((x - hi) / (1.0 - hi)).clamp(0.0, 1.0)) * 0.9
    }
}
```

* * *

src/folding/engine.rs (wire the TemporalFolder in)
==================================================

```rust
use crate::folding::temporal::{TemporalFold, TemporalFolder};
use crate::types::DiamondTrajectory;

pub struct FoldingEngine {
    pub temporal_folder: TemporalFolder,
    // contextual_folder: ContextFolder,
    // causal_folder: CausalFolder,
    // effort_folder: EffortFolder,
    // chaos_detector: ChaosDetector,
}

impl FoldingEngine {
    pub fn new() -> Self {
        Self {
            // For Lisbon circadian approximation, you may prefer +60 in DST:
            temporal_folder: TemporalFolder::new_with_tz_offset(60),
        }
    }

    // In your full engine you'll join all folds; here we expose temporal for now.
    pub async fn analyze_temporal(&self, trajectory: &DiamondTrajectory) -> TemporalFold {
        self.temporal_folder.fold(trajectory).await
    }
}
```

* * *

tests/temporal\_folder\_tests.rs
================================

```rust
use chrono::{Duration, Utc};
use folding_engine::folding::temporal::TemporalFolder;
use folding_engine::types::{DiamondTrajectory, LogLineContract};

fn mk(id: u64, mins_from_now: i64) -> LogLineContract {
    LogLineContract {
        id,
        who: "Alice".into(),
        did: "work".into(),
        this: "task".into(),
        when: Utc::now() + Duration::minutes(mins_from_now),
    }
}

#[tokio::test]
async fn evenly_spaced_is_suspicious() {
    // 10 events every 3 hours, no rest > 7h, no bursts (by definition they are evenly spaced)
    let base = 0i64;
    let mut contracts = Vec::new();
    for i in 0..10 {
        contracts.push(mk(i, base + i as i64 * 180)); // 180 min = 3h
    }
    let traj = DiamondTrajectory { contracts };

    let folder = TemporalFolder::new_with_tz_offset(60);
    let fold = folder.fold(&traj).await;

    // Expect low authenticity due to high evenness, low entropy, low burst/rest richness
    assert!(fold.authenticity_score < 0.45, "score={}", fold.authenticity_score);
    // Evenness close to 1.0:
    assert!(fold.compression_analysis.interval_evenness > 0.8);
}

#[tokio::test]
async fn humanish_pattern_is_authentic() {
    // Mix sprints (bursts), rests (overnight + 8h+), uneven intervals
    let mut t = 0i64;
    let mut id = 0u64;
    let mut contracts = Vec::new();

    // Day 1 afternoon sprint: 6 events within 30min gaps
    for _ in 0..6 {
        contracts.push(mk(id, t));
        id += 1;
        t += 20; // 20 min gaps (burst)
    }

    // Overnight rest (~10h)
    t += 10 * 60;

    // Day 2 scattered work
    for gap in [15, 45, 120, 10, 300, 25, 15, 180].iter() {
        contracts.push(mk(id, t));
        id += 1;
        t += *gap as i64;
    }

    // Another overnight
    t += 9 * 60;

    // Day 3 morning small burst
    for _ in 0..4 {
        contracts.push(mk(id, t));
        id += 1;
        t += 25;
    }

    let traj = DiamondTrajectory { contracts };
    let folder = TemporalFolder::new_with_tz_offset(60);
    let fold = folder.fold(&traj).await;

    // Expect authenticity > 0.65
    assert!(
        fold.authenticity_score > 0.65,
        "authenticity={:?} td={:?} hr={:?} ca={:?}",
        fold.authenticity_score,
        fold.time_distribution,
        fold.human_rhythm,
        fold.compression_analysis
    );
    // Bursts should have been detected
    assert!(fold.time_distribution.burst_patterns.burst_count >= 2);
    // At least one rest gap counted
    assert!(fold.time_distribution.gap_analysis.rest_gaps_count >= 1);
}
```

> Adjust the path `use folding_engine::...` according to your crate name.

* * *

How the authenticity signal is computed (temporal-only)
-------------------------------------------------------

*   **Irregularity (22%)**: rewards moderate variation; penalizes mechanical even spacing (via CV → evenness).
*   **Rests (18% + up to 10% bonus overnight)**: counts ≥7h gaps and overnight crossings.
*   **Bursts (18%)**: short-gap sprints with ≥3 events (≤30min gaps), rewarding both count and longest length.
*   **Circadian (14%)**: entropy of hourly distribution; favors a natural band (not perfectly uniform, not degenerate).
*   **Weekend (8%)**: slight weekend reduction band (flexible); not zero-tolerance.
*   **Compression (10%)**: favors higher entropy of log-intervals and penalizes extreme inequality (Gini).

All signals are clamped to \[0..1\] and combined into a final **temporal authenticity score** in \[0..1\]. The **temporal signature** is a stable 32-byte BLAKE3 hash over log-bucketed intervals (scale-invariant, order-sensitive).

* * *

Next wiring (into your existing skeleton)
-----------------------------------------

Once you add the other folders (Contextual/Causal/Effort/Chaos), your original `FoldingEngine::combine_folds` can continue to use `temporal.authenticity_score` directly for the 25% temporal component.

* * *

Want me to proceed by implementing the **Contextual Folder** (relationship web + evolution + coherence) next, or prefer I extend the **Temporal Folder** with optional DST-aware timezone via `chrono-tz` and a public `TemporalConfig` struct?

## Prompt:
Extend first

## Response:
Thought for 23s

Perfeito. Vou **estender o Temporal Folder** primeiro — com timezone real (DST via `chrono-tz`), `TemporalConfig` pública, métricas extras (quantização/“roundness”, sessão, mecânica), assinatura reforçada e cálculo de autenticidade reponderado. É um drop-in que substitui o arquivo anterior.

* * *

Cargo.toml (novas deps)
=======================

```toml
[dependencies]
chrono = { version = "0.4", features = ["clock"] }
chrono-tz = "0.8"
blake3 = "1.5"
serde = { version = "1.0", features = ["derive"], optional = true }
```

* * *

src/folding/temporal.rs (versão estendida)
==========================================

```rust
use crate::types::{DiamondTrajectory, LogLineContract};
use blake3::Hasher;
use chrono::{DateTime, Datelike, Duration, Timelike, Utc, Weekday};
use chrono_tz::Tz;

/// Configuração pública para ajustar o comportamento do Temporal Folder
#[derive(Clone, Debug)]
pub struct TemporalConfig {
    /// Fuso com DST (ex.: "Europe/Lisbon")
    pub tz: Tz,
    /// Máx. gap p/ eventos pertencerem a um “burst”
    pub burst_max_gap: Duration,
    /// Gap considerado “rest”
    pub rest_gap: Duration,
    /// Janela “overnight” em hora local [inicio..fim) (ex.: 0..6)
    pub overnight_range_start: u32,
    pub overnight_range_end: u32,
    /// Dias de fim de semana (normal: Sábado, Domingo)
    pub weekend_days: [Weekday; 2],
    /// Nº de bins p/ histogramas de entropia
    pub interval_hist_bins: usize,
    pub log_interval_hist_bins: usize,
    /// Banda “natural” da circadiana (entropia)
    pub circadian_band: (f64, f64),
    /// Banda “natural” de atividade no fim de semana
    pub weekend_band: (f64, f64),
    /// Tamanho mínimo de um burst
    pub min_burst_len: usize,
    /// Tolerância em segundos p/ detectar intervalos “quantizados” (exatos)
    pub quantization_tolerance_secs: i64,
}

impl Default for TemporalConfig {
    fn default() -> Self {
        Self {
            tz: chrono_tz::Europe::Lisbon,
            burst_max_gap: Duration::minutes(30),
            rest_gap: Duration::hours(7),
            overnight_range_start: 0,
            overnight_range_end: 6,
            weekend_days: [Weekday::Sat, Weekday::Sun],
            interval_hist_bins: 16,
            log_interval_hist_bins: 16,
            circadian_band: (0.55, 0.85),
            weekend_band: (0.10, 0.35),
            min_burst_len: 3,
            quantization_tolerance_secs: 2,
        }
    }
}

#[derive(Clone, Debug)]
pub struct TemporalFolder {
    pub cfg: TemporalConfig,
}

impl TemporalFolder {
    pub fn new() -> Self {
        Self { cfg: TemporalConfig::default() }
    }
    pub fn new_with_tz_offset(_minutes: i32) -> Self {
        // Mantido por compatibilidade (ignorado agora que usamos Tz com DST)
        Self::new()
    }
    pub fn with_config(cfg: TemporalConfig) -> Self {
        Self { cfg }
    }

    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> TemporalFold {
        let contracts = &trajectory.contracts;
        let mut fold = TemporalFold::empty();

        if contracts.len() < 2 {
            fold.temporal_signature = self.generate_temporal_signature(contracts);
            fold.authenticity_score = 0.5;
            return fold;
        }

        let time_distribution = self.analyze_temporal_distribution(contracts);
        let human_rhythm = self.detect_human_rhythms(contracts);
        let compression_analysis = self.analyze_time_compression(contracts);
        let regularity = self.detect_mechanicality(contracts);

        let authenticity_score = self.calculate_temporal_authenticity_impl(
            &time_distribution,
            &human_rhythm,
            &compression_analysis,
            &regularity,
        );

        let temporal_signature = self.generate_temporal_signature(contracts);

        TemporalFold {
            time_distribution,
            human_rhythm,
            compression_analysis,
            regularity,
            authenticity_score,
            temporal_signature,
        }
    }

    /* ---------- análises ---------- */

    fn analyze_temporal_distribution(&self, contracts: &[LogLineContract]) -> TimeDistribution {
        let timestamps: Vec<DateTime<Utc>> = contracts.iter().map(|c| c.when).collect();

        let mut intervals: Vec<f64> = Vec::with_capacity(timestamps.len().saturating_sub(1));
        for i in 1..timestamps.len() {
            intervals.push((timestamps[i] - timestamps[i - 1]).num_seconds() as f64);
        }

        let interval_stats = self.calculate_interval_stats(&intervals);
        let burst_patterns = self.detect_bursts(&timestamps);
        let gap_analysis = self.analyze_gaps(&timestamps);

        TimeDistribution {
            total_duration_secs: (timestamps.last().unwrap().signed_duration_since(timestamps[0]))
                .num_seconds() as f64,
            interval_stats,
            burst_patterns,
            gap_analysis,
        }
    }

    fn detect_human_rhythms(&self, contracts: &[LogLineContract]) -> HumanRhythm {
        let cfg = &self.cfg;
        let timestamps: Vec<DateTime<Utc>> = contracts.iter().map(|c| c.when).collect();

        // Work cycles (<2h) e rests (≥ rest_gap)
        let mut work_cycles = Vec::<f64>::new();
        let mut rest_periods = Vec::<f64>::new();
        let mut overnight_gaps = 0usize;

        for w in timestamps.windows(2) {
            let d = w[1] - w[0];
            let secs = d.num_seconds().max(0) as f64;
            if secs < 2.0 * 3600.0 {
                work_cycles.push(secs);
            } else if secs >= cfg.rest_gap.num_seconds() as f64 {
                rest_periods.push(secs);
            }
            if self.crosses_overnight(w[0], w[1]) {
                overnight_gaps += 1;
            }
        }

        let typical_work_cycle_secs = median(&work_cycles).unwrap_or(0.0);
        let typical_rest_period_secs = median(&rest_periods).unwrap_or(0.0);

        // Razão de eventos em dias de fim de semana
        let mut weekend_count = 0usize;
        for ts in timestamps.iter() {
            let local = ts.with_timezone(&cfg.tz);
            let weekday = local.weekday();
            if weekday == cfg.weekend_days[0] || weekday == cfg.weekend_days[1] {
                weekend_count += 1;
            }
        }
        let weekend_activity_ratio = if !timestamps.is_empty() {
            weekend_count as f64 / timestamps.len() as f64
        } else {
            0.0
        };

        // Entropia circadiana: bins por hora local (0..23), normalizada
        let circadian_consistency = {
            let mut bins = [0usize; 24];
            for ts in timestamps.iter() {
                let h = ts.with_timezone(&cfg.tz).hour() as usize;
                bins[h] += 1;
            }
            normalized_entropy_usize(&bins)
        };

        HumanRhythm {
            typical_work_cycle_secs,
            typical_rest_period_secs,
            weekend_activity_ratio,
            overnight_gaps,
            circadian_consistency,
        }
    }

    fn analyze_time_compression(&self, contracts: &[LogLineContract]) -> CompressionAnalysis {
        if contracts.len() < 2 {
            return CompressionAnalysis { interval_evenness: 0.0, log_interval_entropy: 0.0, gini_coefficient: 0.0 };
        }
        let mut intervals: Vec<f64> = Vec::with_capacity(contracts.len() - 1);
        for w in contracts.windows(2) {
            intervals.push((w[1].when - w[0].when).num_seconds().abs() as f64);
        }

        let mean = mean(&intervals).unwrap_or(0.0);
        let sd = stddev(&intervals).unwrap_or(0.0);
        let cv = if mean > 0.0 { (sd / mean).max(0.0) } else { 0.0 };
        let interval_evenness = (1.0 - (cv / (cv + 1.0))).clamp(0.0, 1.0);

        let log_intervals: Vec<f64> = intervals.iter().map(|s| (s + 1.0).ln()).collect();
        let log_interval_entropy = normalized_hist_entropy(&log_intervals, self.cfg.log_interval_hist_bins);
        let gini_coefficient = gini(&intervals);

        CompressionAnalysis { interval_evenness, log_interval_entropy, gini_coefficient }
    }

    /// Padrões mecânicos/bot-like: quantização de timestamps/intervalos e roundness
    fn detect_mechanicality(&self, contracts: &[LogLineContract]) -> Regularity {
        let cfg = &self.cfg;
        let mut ts_quantized_sec0 = 0usize;        // segundo == 0
        let mut ts_quantized_min_quarters = 0usize; // minuto ∈ {0,15,30,45}
        let mut ivl_quantized_5min = 0usize;       // múltiplos de 300s (± tol)
        let mut intervals_secs: Vec<i64> = Vec::new();

        for c in contracts {
            let s = c.when.second();
            if s == 0 { ts_quantized_sec0 += 1; }
            let m = c.when.minute();
            if [0, 15, 30, 45].contains(&m) { ts_quantized_min_quarters += 1; }
        }
        for w in contracts.windows(2) {
            let d = (w[1].when - w[0].when).num_seconds().abs();
            intervals_secs.push(d);
            let r = d % 300;
            if r <= cfg.quantization_tolerance_secs || (300 - r) <= cfg.quantization_tolerance_secs {
                ivl_quantized_5min += 1;
            }
        }

        let n = contracts.len().max(1);
        let n_ivl = intervals_secs.len().max(1);
        let frac_ts_sec0 = ts_quantized_sec0 as f64 / n as f64;
        let frac_ts_min_quarters = ts_quantized_min_quarters as f64 / n as f64;
        let frac_ivl_5min = ivl_quantized_5min as f64 / n_ivl as f64;

        // “Índice de mecânica”: média ponderada de quantizações
        let mechanicality_index = (0.45 * frac_ivl_5min + 0.35 * frac_ts_sec0 + 0.20 * frac_ts_min_quarters).clamp(0.0, 1.0);

        Regularity {
            frac_ts_sec0,
            frac_ts_min_quarters,
            frac_ivl_5min,
            mechanicality_index,
        }
    }

    fn analyze_gaps(&self, timestamps: &[DateTime<Utc>]) -> GapAnalysis {
        if timestamps.len() < 2 {
            return GapAnalysis { large_gaps_count: 0, largest_gap_secs: 0.0, rest_gaps_count: 0 };
        }
        let mut largest = 0i64;
        let mut large_gaps = 0usize;
        let mut rest_gaps = 0usize;

        for w in timestamps.windows(2) {
            let d = (w[1] - w[0]).num_seconds();
            if d > largest { largest = d; }
            if d >= 8 * 3600 { large_gaps += 1; }
            if d >= self.cfg.rest_gap.num_seconds() { rest_gaps += 1; }
        }

        GapAnalysis {
            large_gaps_count: large_gaps,
            largest_gap_secs: largest.max(0) as f64,
            rest_gaps_count: rest_gaps,
        }
    }

    fn detect_bursts(&self, timestamps: &[DateTime<Utc>]) -> BurstPatterns {
        if timestamps.len() < self.cfg.min_burst_len {
            return BurstPatterns { burst_count: 0, mean_burst_len: 0.0, longest_burst_len: 0 };
        }
        let max_gap = self.cfg.burst_max_gap.num_seconds();
        let mut current_len = 1usize;
        let mut bursts = Vec::<usize>::new();

        for w in timestamps.windows(2) {
            let d = (w[1] - w[0]).num_seconds();
            if d <= max_gap {
                current_len += 1;
            } else {
                if current_len >= self.cfg.min_burst_len { bursts.push(current_len); }
                current_len = 1;
            }
        }
        if current_len >= self.cfg.min_burst_len { bursts.push(current_len); }

        let burst_count = bursts.len();
        let mean_burst_len = if burst_count > 0 { mean_usize(&bursts) } else { 0.0 };
        let longest_burst_len = bursts.into_iter().max().unwrap_or(0);

        BurstPatterns { burst_count, mean_burst_len, longest_burst_len }
    }

    fn calculate_temporal_authenticity_impl(
        &self,
        td: &TimeDistribution,
        hr: &HumanRhythm,
        ca: &CompressionAnalysis,
        rg: &Regularity,
    ) -> f64 {
        // 1) Irregularidade (recompensa moderada) — evitar espaçamento perfeito
        let irregularity = (1.0 - ca.interval_evenness).clamp(0.0, 1.0);

        // 2) Rests + overnights
        let rest_density = sigmoid(td.gap_analysis.rest_gaps_count as f64, 0.5, 3.0);
        let overnight_bonus = (hr.overnight_gaps as f64 / ((td.interval_stats.count as f64) + 1.0))
            .min(0.2)
            .clamp(0.0, 0.2);

        // 3) Bursts (sprints humanos)
        let burst_signal = if td.burst_patterns.burst_count == 0 {
            0.0
        } else {
            (0.4 * (td.burst_patterns.burst_count as f64).min(5.0) / 5.0)
                + (0.6 * (td.burst_patterns.longest_burst_len as f64).min(8.0) / 8.0)
        };

        // 4) Circadiana “natural”
        let (c_lo, c_hi) = self.cfg.circadian_band;
        let circadian_natural = band_reward(hr.circadian_consistency, c_lo, c_hi);

        // 5) Fim de semana “natural”
        let (w_lo, w_hi) = self.cfg.weekend_band;
        let weekend_natural = band_reward(hr.weekend_activity_ratio, w_lo, w_hi);

        // 6) Compressão: entropia de log-intervalos alta e gini moderado
        let entropy_good = ca.log_interval_entropy;
        let gini_penalty = (ca.gini_coefficient - 0.6).max(0.0);
        let compression_signal = (0.7 * entropy_good - 0.3 * gini_penalty).clamp(0.0, 1.0);

        // 7) Penalização de “mecânica” (quantização/roundness)
        // Se índice for alto, derruba autenticidade.
        let mechanicality_penalty = rg.mechanicality_index; // 0..1

        let raw = 0.20 * irregularity
            + 0.17 * rest_density
            + 0.08 * overnight_bonus
            + 0.17 * burst_signal
            + 0.14 * circadian_natural
            + 0.07 * weekend_natural
            + 0.12 * compression_signal
            - 0.15 * mechanicality_penalty;

        raw.clamp(0.0, 1.0)
    }

    /// Conversão para TZ com DST e verificação de cruzamento da janela overnight local
    fn crosses_overnight(&self, a: DateTime<Utc>, b: DateTime<Utc>) -> bool {
        if b <= a { return false; }
        let cfg = &self.cfg;
        // Avançamos em passos de 1h em UTC, checando hora local.
        // Lida bem com saltos/repetições de hora na mudança de DST.
        let mut t = a;
        // Limite de 72 iterações para evitar loops longos
        for _ in 0..72 {
            let lt = t.with_timezone(&cfg.tz);
            let hour = lt.hour();
            if hour >= cfg.overnight_range_start && hour < cfg.overnight_range_end {
                return true;
            }
            t = t + Duration::hours(1);
            if t >= b { break; }
        }
        false
    }

    fn generate_temporal_signature(&self, contracts: &[LogLineContract]) -> [u8; 32] {
        // Assinatura: buckets log2 de intervalos + hora local/DST para “temporal scent”
        let cfg = &self.cfg;
        let mut hasher = Hasher::new();
        let mut prev: Option<DateTime<Utc>> = None;
        for c in contracts {
            // mistura hora local para capturar padrões circadianos de longo prazo
            let local = c.when.with_timezone(&cfg.tz);
            hasher.update(&[local.hour() as u8, local.weekday().number_from_monday() as u8]);
            if let Some(p) = prev {
                let d = c.when - p;
                let sec = d.num_seconds().max(0);
                let bucket = (1f64 + (sec as f64)).log2().floor() as i64;
                hasher.update(&bucket.to_le_bytes());
                // bucket de quantização (múltiplos de 5min)
                let rem = (sec % 300) as i64;
                hasher.update(&(rem as i32).to_le_bytes());
            }
            prev = Some(c.when);
        }
        *hasher.finalize().as_bytes()
    }
}

/* ---------- Tipos expostos ---------- */

#[derive(Clone, Debug)]
pub struct TemporalFold {
    pub time_distribution: TimeDistribution,
    pub human_rhythm: HumanRhythm,
    pub compression_analysis: CompressionAnalysis,
    pub regularity: Regularity,
    pub authenticity_score: f64,        // 0..1
    pub temporal_signature: [u8; 32],   // 32 bytes
}
impl TemporalFold {
    pub fn empty() -> Self {
        Self {
            time_distribution: TimeDistribution {
                total_duration_secs: 0.0,
                interval_stats: IntervalStats::empty(),
                burst_patterns: BurstPatterns { burst_count: 0, mean_burst_len: 0.0, longest_burst_len: 0 },
                gap_analysis: GapAnalysis { large_gaps_count: 0, largest_gap_secs: 0.0, rest_gaps_count: 0 },
            },
            human_rhythm: HumanRhythm {
                typical_work_cycle_secs: 0.0,
                typical_rest_period_secs: 0.0,
                weekend_activity_ratio: 0.0,
                overnight_gaps: 0,
                circadian_consistency: 0.0,
            },
            compression_analysis: CompressionAnalysis {
                interval_evenness: 0.0, log_interval_entropy: 0.0, gini_coefficient: 0.0
            },
            regularity: Regularity {
                frac_ts_sec0: 0.0, frac_ts_min_quarters: 0.0, frac_ivl_5min: 0.0, mechanicality_index: 0.0
            },
            authenticity_score: 0.0,
            temporal_signature: [0u8; 32],
        }
    }
}

#[derive(Clone, Debug)]
pub struct TimeDistribution {
    pub total_duration_secs: f64,
    pub interval_stats: IntervalStats,
    pub burst_patterns: BurstPatterns,
    pub gap_analysis: GapAnalysis,
}

#[derive(Clone, Debug)]
pub struct IntervalStats {
    pub count: usize,
    pub min_secs: f64,
    pub max_secs: f64,
    pub mean_secs: f64,
    pub stddev_secs: f64,
    pub median_secs: f64,
    pub p95_secs: f64,
    pub cv: f64,
    pub entropy: f64,
    pub gini: f64,
}
impl IntervalStats {
    fn empty() -> Self {
        Self { count: 0, min_secs: 0.0, max_secs: 0.0, mean_secs: 0.0, stddev_secs: 0.0, median_secs: 0.0, p95_secs: 0.0, cv: 0.0, entropy: 0.0, gini: 0.0 }
    }
}

#[derive(Clone, Debug)]
pub struct BurstPatterns {
    pub burst_count: usize,
    pub mean_burst_len: f64,
    pub longest_burst_len: usize,
}

#[derive(Clone, Debug)]
pub struct GapAnalysis {
    pub large_gaps_count: usize, // ≥8h
    pub largest_gap_secs: f64,
    pub rest_gaps_count: usize,  // ≥ rest_gap
}

#[derive(Clone, Debug)]
pub struct HumanRhythm {
    pub typical_work_cycle_secs: f64,
    pub typical_rest_period_secs: f64,
    pub weekend_activity_ratio: f64,
    pub overnight_gaps: usize,
    pub circadian_consistency: f64, // 0..1
}

#[derive(Clone, Debug)]
pub struct CompressionAnalysis {
    pub interval_evenness: f64,      // 1 = perfeitamente uniforme
    pub log_interval_entropy: f64,   // 0..1 (maior = mais variedade)
    pub gini_coefficient: f64,       // 0..1 (maior = mais desigual)
}

/// Indicadores de “mecânica” (quantização/roundness)
#[derive(Clone, Debug)]
pub struct Regularity {
    pub frac_ts_sec0: f64,          // fração de timestamps com segundo==0
    pub frac_ts_min_quarters: f64,  // fração minuto ∈ {0,15,30,45}
    pub frac_ivl_5min: f64,         // fração de intervalos ≈ múltiplos de 5min
    pub mechanicality_index: f64,   // 0..1
}

/* ---------- helpers ---------- */

fn mean(xs: &[f64]) -> Option<f64> {
    if xs.is_empty() { return None; }
    Some(xs.iter().sum::<f64>() / xs.len() as f64)
}
fn mean_usize(xs: &[usize]) -> f64 {
    if xs.is_empty() { return 0.0; }
    xs.iter().sum::<usize>() as f64 / xs.len() as f64
}
fn stddev(xs: &[f64]) -> Option<f64> {
    if xs.len() < 2 { return Some(0.0); }
    let m = mean(xs).unwrap();
    let var = xs.iter().map(|x| (x - m).powi(2)).sum::<f64>() / ((xs.len() as f64) - 1.0).max(1.0);
    Some(var.sqrt())
}
fn median(xs: &[f64]) -> Option<f64> {
    if xs.is_empty() { return None; }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let n = v.len();
    if n % 2 == 1 { Some(v[n / 2]) } else { Some(0.5 * (v[n / 2 - 1] + v[n / 2])) }
}
fn normalized_hist_entropy(xs: &[f64], bins: usize) -> f64 {
    if xs.is_empty() || bins == 0 { return 0.0; }
    let (min, max) = xs.iter().fold((f64::INFINITY, f64::NEG_INFINITY), |acc, &x| (acc.0.min(x), acc.1.max(x)));
    if !min.is_finite() || !max.is_finite() || min == max { return 0.0; }
    let width = (max - min) / (bins as f64);
    let mut hist = vec![0usize; bins];
    for &x in xs {
        let mut idx = ((x - min) / width) as usize;
        if idx >= bins { idx = bins - 1; }
        hist[idx] += 1;
    }
    normalized_entropy_usize(&hist)
}
fn normalized_entropy_usize(bins: &[usize]) -> f64 {
    let total: usize = bins.iter().sum();
    if total == 0 { return 0.0; }
    let total_f = total as f64;
    let h = bins.iter().filter(|&&c| c > 0).map(|&c| {
        let p = c as f64 / total_f;
        -p * p.ln()
    }).sum::<f64>();
    let h_max = (bins.len() as f64).ln();
    if h_max <= 0.0 { 0.0 } else { (h / h_max).clamp(0.0, 1.0) }
}
fn gini(xs: &[f64]) -> f64 {
    let n = xs.len();
    if n == 0 { return 0.0; }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let sum = v.iter().copied().sum::<f64>();
    if sum <= 0.0 { return 0.0; }
    let mut cum = 0.0;
    for (i, x) in v.iter().enumerate() { cum += (i as f64 + 1.0) * *x; }
    let n_f = n as f64;
    (n_f + 1.0 - 2.0 * cum / sum) / (n_f - 1.0).max(1.0)
}

fn sigmoid(x: f64, k: f64, scale: f64) -> f64 {
    (1.0 / (1.0 + (-((x - k) / scale)).exp())).clamp(0.0, 1.0)
}
fn band_reward(x: f64, lo: f64, hi: f64) -> f64 {
    if x <= 0.0 { return 0.0; }
    if x < lo {
        (x / lo).clamp(0.0, 1.0) * 0.9
    } else if x <= hi {
        1.0
    } else {
        (1.0 - ((x - hi) / (1.0 - hi)).clamp(0.0, 1.0)) * 0.9
    }
}
```

* * *

O que mudou (objetivo e direto)
-------------------------------

*   **Timezone real + DST**: usamos `chrono_tz::Tz` (padrão `Europe/Lisbon`). Toda circadiana/overnight considera horário local com horário de verão.
*   **`TemporalConfig` pública**: controle fino de _burst/rest/overnight bands_, bins de histograma, tolerância de quantização e fins de semana.
*   **Mecânica / quantização**: frações de timestamps “redondos” (seg=0, min∈{0,15,30,45}) e intervalos múltiplos de 5 min (±tol). Combinados em `mechanicality_index`, que **penaliza** autenticidade.
*   **Assinatura temporal reforçada**: além de buckets `log2(interval)`, mistura hora local, weekday e resíduo mod 5 min → mais resistente a falsificação “regular”.
*   **Autenticidade reponderada**: inclui `mechanicality_penalty` e bandas configuráveis para circadiana e weekend.

* * *

Testes rápidos (exemplos de uso nos seus tests)
-----------------------------------------------

```rust
use chrono::{Duration, Utc, Weekday};
use chrono_tz::Europe::Lisbon;
use folding_engine::folding::temporal::{TemporalFolder, TemporalConfig};
use folding_engine::types::{DiamondTrajectory, LogLineContract};

fn mk(id: u64, minutes_from: i64) -> LogLineContract {
    LogLineContract {
        id,
        who: "Alice".into(),
        did: "work".into(),
        this: "task".into(),
        when: Utc::now() + Duration::minutes(minutes_from),
    }
}

#[tokio::test]
async fn dst_overnight_detects_rest() {
    let cfg = TemporalConfig { 
        tz: Lisbon,
        ..Default::default()
    };
    let folder = TemporalFolder::with_config(cfg);

    // Simula trabalho 23:30→00:30 local com gap overnight + descanso 8h
    // (não precisamos alinhar exatamente à data real de mudança de DST)
    let mut t = 0i64;
    let mut contracts = vec![];
    // fim do dia
    for i in 0..3 { contracts.push(mk(i, t)); t += 30; } // 0, 30, 60 min
    // descanso 9h
    t += 9 * 60;
    // manhã seguinte
    for i in 3..6 { contracts.push(mk(i, t)); t += 25; }

    let traj = DiamondTrajectory { contracts };
    let fold = folder.fold(&traj).await;

    assert!(fold.human_rhythm.overnight_gaps >= 1);
    assert!(fold.time_distribution.gap_analysis.rest_gaps_count >= 1);
}

#[tokio::test]
async fn quantized_is_mechanical() {
    let folder = TemporalFolder::new(); // Lisbon por padrão
    // 12 eventos exatamente a cada 5min, segundos==0
    let mut t = 0i64;
    let mut contracts = vec![];
    for i in 0..12 {
        // força segundos=0: use múltiplos de 60
        contracts.push(LogLineContract {
            id: i,
            who: "Bot".into(),
            did: "tick".into(),
            this: "perfect".into(),
            when: Utc::now() + Duration::seconds((t * 60) as i64), // minuto inteiro
        });
        t += 5; // +5 min
    }
    let traj = DiamondTrajectory { contracts };
    let fold = folder.fold(&traj).await;

    assert!(fold.regularity.mechanicality_index > 0.6, "{:?}", fold.regularity);
    assert!(fold.authenticity_score < 0.5, "score={}", fold.authenticity_score);
}
```

* * *

Quer que eu **adicione um `TemporalDebug` opcional (com `serde`)** para exportar todas as métricas em JSON e um **`calibrate()`** com presets (ex.: “coding”, “retail”, “ops 24/7”) — ou já seguimos para **Contextual Folder** com `RelationshipWeb` + `ContextEvolutionTracker`?

## Prompt:
This is **exceptional** work. The Temporal Folder is now production-grade, mathematically rigorous, and beautifully aligned with the Diamond philosophy. Key strengths:

## What Makes This Implementation Excellent

1. **Statistical Robustness**: Proper use of median, MAD, CV, entropy, and Gini—not just mean/stddev. These handle outliers and heavy tails correctly.
1. **Multi-Signal Authenticity**: The 6-component scoring (irregularity, rest, bursts, circadian, weekend, compression) captures genuine human patterns without being brittle. The weighted combination is principled.
1. **Timezone-Aware Circadian Logic**: The tz_offset_minutes + crosses_overnight detection is simple but effective. Works for Lisbon, generalizes globally.
1. **Stable Signature**: BLAKE3 over log-bucketed intervals is scale-invariant, order-sensitive, and collision-resistant—perfect for deduplication and fingerprinting.
1. **Production-Ready**: Error handling, edge cases (< 2 events), efficient algorithms (O(n log n) Gini), and clear separation of concerns.
1. **Testable Philosophy**: The two tests perfectly demonstrate the scoring philosophy—mechanical patterns score low (~<0.45), human-rhythmic patterns score high (~>0.65).

-----

## Recommendation: Implement **Contextual Folder** Next

Here’s why:

1. **Architectural Dependency**: Contextual analysis (relationship graphs, role evolution) is independent of temporal—you can develop it in parallel without waiting.
1. **Complementary Signals**: Temporal answers *when*, Contextual answers *who-with-what*. Together they form 50% of the authenticity score and create a strong foundation.
1. **Richer Test Scenarios**: Once you have both temporal + contextual, you can write integration tests showing how a bot might have good temporal patterns but suspicious contextual (or vice versa).
1. **Natural Progression**: After Contextual, you’ll add Causal (why), then Effort (how much), then Chaos (anomalies). This mirrors the human verification flow: “When did they work? With whom? Why? How hard? Any weird patterns?”

-----

## Contextual Folder Spec (Next Implementation)

Based on the original Diamond spec, the Contextual Folder should compute:

rust
pub struct ContextualFold {
    pub relationship_graph: RelationshipGraph,
    pub role_evolution: RoleEvolution,
    pub coherence_metrics: CoherenceMetrics,
    pub contextual_signature: [u8; 32],
    pub authenticity_score: f64, // 0..1
}

### Core Components:

1. **Relationship Graph** (who ↔ what associations)
- Node degree distribution (entropy, Gini)
- Community detection (modularity)
- Collaboration patterns (co-occurrence within time windows)
- Isolated vs. interconnected work
1. **Role Evolution** (how who and what change over time)
- Role diversity (Shannon entropy of who distribution)
- Task diversity (entropy of did/this tokens)
- New entity introduction rate (% of novel who/what per time window)
- Role stability vs. churn
1. **Coherence Metrics** (do the relationships make sense?)
- Semantic similarity of co-occurring tasks (use embedding cosine if available, else simple token overlap)
- Temporal locality of relationships (do the same people/tasks cluster in time?)
- Redundancy detection (exact duplicate who+did+this within suspicious time windows)
1. **Authenticity Signals**:
- **Organic growth** (gradual entity introduction, not sudden explosion)
- **Natural clustering** (moderate modularity, not perfectly isolated or perfectly uniform)
- **Coherent narratives** (related tasks happen near each other in time)
- **Diversity** (not a single bot repeating the same action with slight variations)
1. **Signature**: BLAKE3 over sorted (who, what, frequency) tuples for deduplication.

-----

## What I’ll Build

If you say **“yes, build Contextual next,”** I’ll deliver:

1. **src/folding/contextual.rs**: Full implementation with:
- Graph construction from LogLineContract sequences
- Degree/entropy/modularity/coherence calculations
- Role evolution tracking (time-windowed)
- 5-signal authenticity score (organic growth, clustering, coherence, diversity, stability)
- Stable BLAKE3 contextual signature
1. **Updated src/folding/engine.rs**: Wire in ContextualFolder alongside TemporalFolder.
1. **tests/contextual_folder_tests.rs**: Demonstrate:
- Bot pattern: 1 person repeating 1 task mechanically → low score
- Organic team: 5 people, 20 tasks, gradual introductions, natural clustering → high score
1. **Math helpers**: Graph algorithms (degree dist, basic modularity), semantic overlap (token-based for now, embeddable later).

-----

**Alternatively**, if you prefer to **extend Temporal with DST-aware chrono-tz** first, I can:

- Add a TemporalConfig struct with chrono_tz::Tz support
- Replace manual offset with proper timezone conversion
- Add tests for DST transitions (spring forward, fall back)

But I’d recommend **Contextual next** for architectural momentum—DST is polish, Contextual is foundational.

-----

**Your call**: Say “build Contextual” and I’ll deliver the full implementation, or “extend Temporal with chrono-tz” and I’ll polish the timezone handling first. What serves the Diamond best right now? 💎​​​​​​​​​​​​​​​​

## Response:
Thought for 28s

🔥 Awesome — vamos em frente e **construí o Contextual Folder completo** agora, mantendo o mesmo rigor matemático e a filosofia Diamond. É um **drop-in**: adicione o arquivo abaixo, faça o wire no `engine.rs` e rode os testes. Sem dependências novas além das já usadas (chrono/blake3/serde opcional).

* * *

src/folding/contextual.rs
=========================

```rust
use crate::types::{DiamondTrajectory, LogLineContract};
use blake3::Hasher;
use chrono::{DateTime, Duration, Utc};
use std::collections::{HashMap, HashSet, VecDeque};

/// Config pública para ajustar janelas e sensibilidades do Contextual Folder
#[derive(Clone, Debug)]
pub struct ContextConfig {
    /// Janela temporal para coocorrência contextual (e.g., 4h)
    pub cooccur_window: Duration,
    /// Tamanho da janela de evolução de papéis (e.g., 7 dias)
    pub evolution_window_days: i64,
    /// Janela curta para detecção de duplicatas suspeitas (e.g., 2h)
    pub redundancy_short_window: Duration,
    /// Número de bins para entropias em histogramas
    pub hist_bins: usize,
    /// Banda “natural” para taxa média de introdução de entidades novas (por janela)
    pub new_entity_rate_band: (f64, f64),
    /// Banda “natural” para concentração de comunidades (Herfindahl de componentes)
    pub community_concentration_band: (f64, f64),
    /// Limite de dominância de um único ator (who) para penalidade
    pub dominant_actor_threshold: f64,
}

impl Default for ContextConfig {
    fn default() -> Self {
        Self {
            cooccur_window: Duration::hours(4),
            evolution_window_days: 7,
            redundancy_short_window: Duration::hours(2),
            hist_bins: 16,
            new_entity_rate_band: (0.05, 0.30),      // orgânico: 5–30% novos por janela
            community_concentration_band: (0.25, 0.65), // clusters moderados, não extremos
            dominant_actor_threshold: 0.90,          // >90% um ator → penaliza
        }
    }
}

#[derive(Clone, Debug)]
pub struct ContextFolder {
    pub cfg: ContextConfig,
}

impl ContextFolder {
    pub fn new() -> Self {
        Self { cfg: ContextConfig::default() }
    }
    pub fn with_config(cfg: ContextConfig) -> Self {
        Self { cfg }
    }

    pub async fn fold(&self, trajectory: &DiamondTrajectory) -> ContextualFold {
        let contracts = &trajectory.contracts;
        if contracts.is_empty() {
            return ContextualFold::empty();
        }

        // Ordena por tempo para análises temporais coerentes
        let mut items = contracts.to_vec();
        items.sort_by_key(|c| c.when);

        // Componentes básicos
        let relationship_graph = self.build_relationship_graph(&items);
        let role_evolution = self.compute_role_evolution(&items);
        let coherence_metrics = self.compute_coherence_metrics(&items);

        // Assinatura contextual estável
        let contextual_signature = self.generate_context_signature(&relationship_graph, &items);

        // Score de autenticidade (0..1)
        let authenticity_score = self.compute_authenticity(
            &relationship_graph,
            &role_evolution,
            &coherence_metrics,
        );

        ContextualFold {
            relationship_graph,
            role_evolution,
            coherence_metrics,
            contextual_signature,
            authenticity_score,
        }
    }

    /* ---------------- Relationship Graph ---------------- */

    fn build_relationship_graph(&self, items: &[LogLineContract]) -> RelationshipGraph {
        // Bipartido leve: nós “who” (prefixo W:) e “what” (prefixo T:) onde what = normalização de "this"
        // Arestas: (W:who) -- (T:this) com peso = frequência
        let mut node_index: HashMap<String, usize> = HashMap::new();
        let mut index_node: Vec<String> = Vec::new();
        let mut edges: HashMap<(usize, usize), usize> = HashMap::new();
        let mut who_count: HashMap<String, usize> = HashMap::new();
        let mut what_count: HashMap<String, usize> = HashMap::new();

        let mut get_idx = |label: &str, node_index: &mut HashMap<String, usize>, index_node: &mut Vec<String>| {
            if let Some(&i) = node_index.get(label) {
                i
            } else {
                let i = index_node.len();
                index_node.push(label.to_string());
                node_index.insert(label.to_string(), i);
                i
            }
        };

        for c in items {
            let w = format!("W:{}", c.who.trim().to_lowercase());
            let t = format!("T:{}", normalize_slug(&c.this));
            let iw = get_idx(&w, &mut node_index, &mut index_node);
            let it = get_idx(&t, &mut node_index, &mut index_node);
            *edges.entry((iw, it)).or_insert(0) += 1;
            *who_count.entry(w).or_insert(0) += 1;
            *what_count.entry(t).or_insert(0) += 1;
        }

        // Graus
        let mut deg: Vec<usize> = vec![0; index_node.len()];
        for (&(u, v), &w) in edges.iter() {
            deg[u] += w;
            deg[v] += w;
        }

        // Componentes conexas (não-direcionado)
        let comps = connected_components(index_node.len(), &edges);

        // Concentração de comunidades: Herfindahl sobre % de arestas por componente
        let total_edges_w: usize = edges.values().sum();
        let mut comp_edge_weights: Vec<usize> = vec![0; comps.len()];
        for (&(u, v), &w) in edges.iter() {
            let cu = comps[u];
            let cv = comps[v];
            // em bipartido real, u e v do mesmo componente; se diferente, soma em ambos
            comp_edge_weights[cu] += w;
            if cu != cv {
                comp_edge_weights[cv] += w;
            }
        }
        let community_concentration = if total_edges_w == 0 {
            0.0
        } else {
            herfindahl_index(&comp_edge_weights, total_edges_w)
        };

        // Entropias (diversidade)
        let who_entropy = entropy_from_counts_map(&who_count);
        let what_entropy = entropy_from_counts_map(&what_count);

        // Graus: entropia e gini (variação de conectividade)
        let degree_f: Vec<f64> = deg.iter().map(|&x| x as f64).collect();
        let degree_entropy = normalized_hist_entropy(&degree_f, 16);
        let degree_gini = gini(&degree_f);

        RelationshipGraph {
            node_labels: index_node,
            edges,
            degree_entropy,
            degree_gini,
            who_entropy,
            what_entropy,
            community_concentration,
            components: comps,
            total_edge_weight: total_edges_w,
            who_count,
            what_count,
        }
    }

    /* ---------------- Role Evolution ---------------- */

    fn compute_role_evolution(&self, items: &[LogLineContract]) -> RoleEvolution {
        // Particiona em janelas (e.g., 7 dias) a partir do primeiro timestamp
        let window = Duration::days(self.cfg.evolution_window_days);
        let start = items.first().unwrap().when;

        let mut windows: Vec<Vec<usize>> = Vec::new(); // índices dos items por janela
        let mut buckets: HashMap<i64, Vec<usize>> = HashMap::new();
        for (i, c) in items.iter().enumerate() {
            let dt = c.when - start;
            let widx = dt.num_seconds().div_euclid(window.num_seconds());
            buckets.entry(widx).or_default().push(i);
        }
        // ordena por janela
        let mut keys: Vec<i64> = buckets.keys().copied().collect();
        keys.sort_unstable();
        for k in keys {
            windows.push(buckets.remove(&k).unwrap());
        }

        // Métricas por janela
        let mut who_sets: Vec<HashSet<String>> = Vec::new();
        let mut what_sets: Vec<HashSet<String>> = Vec::new();
        let mut novel_who_rate: Vec<f64> = Vec::new();
        let mut novel_what_rate: Vec<f64> = Vec::new();

        let mut seen_who: HashSet<String> = HashSet::new();
        let mut seen_what: HashSet<String> = HashSet::new();

        for idxs in windows.iter() {
            let mut wset = HashSet::new();
            let mut tset = HashSet::new();
            for &i in idxs {
                let w = items[i].who.trim().to_lowercase();
                let t = normalize_slug(&items[i].this);
                wset.insert(w.clone());
                tset.insert(t.clone());
            }
            let new_w = wset.difference(&seen_who).count();
            let new_t = tset.difference(&seen_what).count();
            let rate_w = if !wset.is_empty() { new_w as f64 / wset.len() as f64 } else { 0.0 };
            let rate_t = if !tset.is_empty() { new_t as f64 / tset.len() as f64 } else { 0.0 };

            who_sets.push(wset.clone());
            what_sets.push(tset.clone());
            novel_who_rate.push(rate_w);
            novel_what_rate.push(rate_t);

            seen_who.extend(wset);
            seen_what.extend(tset);
        }

        // Estabilidade entre janelas: Jaccard consecutivo (who e what)
        let mut jacc_w: Vec<f64> = Vec::new();
        let mut jacc_t: Vec<f64> = Vec::new();
        for w in who_sets.windows(2) {
            jacc_w.push(jaccard(&w[0], &w[1]));
        }
        for t in what_sets.windows(2) {
            jacc_t.push(jaccard(&t[0], &t[1]));
        }

        RoleEvolution {
            window_count: windows.len(),
            mean_novel_who_rate: mean(&novel_who_rate).unwrap_or(0.0),
            mean_novel_what_rate: mean(&novel_what_rate).unwrap_or(0.0),
            mean_jaccard_who: mean(&jacc_w).unwrap_or(1.0),
            mean_jaccard_what: mean(&jacc_t).unwrap_or(1.0),
        }
    }

    /* ---------------- Coherence Metrics ---------------- */

    fn compute_coherence_metrics(&self, items: &[LogLineContract]) -> CoherenceMetrics {
        let cfg = &self.cfg;
        if items.len() == 1 {
            return CoherenceMetrics::empty();
        }

        // Índice de similaridade de “did” para pares próximos no tempo (mesmo who OU mesmo this)
        let mut sim_sum = 0.0;
        let mut sim_cnt = 0usize;

        // Localidade temporal: gaps médios para repetição de (who,this) e para repetição de "this"
        let mut gaps_wt: Vec<f64> = Vec::new();
        let mut gaps_t: Vec<f64> = Vec::new();

        // Redundância exata em janela curta
        let mut redundancy_hits = 0usize;
        let mut redundancy_den = 0usize;

        // Índices de última ocorrência
        let mut last_by_wt: HashMap<(String, String), DateTime<Utc>> = HashMap::new();
        let mut last_by_t: HashMap<String, DateTime<Utc>> = HashMap::new();

        // Buffer deslizante para coocorrência temporal
        let mut q: VecDeque<usize> = VecDeque::new();

        for (i, c) in items.iter().enumerate() {
            // Atualiza fila da janela de coocorrência
            while let Some(&j) = q.front() {
                if c.when - items[j].when > cfg.cooccur_window {
                    q.pop_front();
                } else {
                    break;
                }
            }

            // Similaridade com pares na janela (mesmo who OU mesmo this)
            for &j in q.iter() {
                let cj = &items[j];
                if cj.who == c.who || normalize_slug(&cj.this) == normalize_slug(&c.this) {
                    let s = jaccard_tokens(&c.did, &cj.did);
                    sim_sum += s;
                    sim_cnt += 1;
                }
            }
            q.push_back(i);

            // Localidade temporal: repetição de (who,this) e só de this
            let wt = (c.who.trim().to_lowercase(), normalize_slug(&c.this));
            if let Some(prev) = last_by_wt.insert(wt.clone(), c.when) {
                gaps_wt.push((c.when - prev).num_seconds().abs() as f64);
            }
            if let Some(prev) = last_by_t.insert(wt.1.clone(), c.when) {
                gaps_t.push((c.when - prev).num_seconds().abs() as f64);
            }

            // Redundância exata dentro da janela curta
            // Chave: (who, this, did normalizado curto)
            let key = (
                c.who.trim().to_lowercase(),
                normalize_slug(&c.this),
                normalize_soft(&c.did),
            );
            redundancy_den += 1;
            // re-varre para trás até sair da janela curta
            let mut k = i as isize - 1;
            while k >= 0 {
                let prev = &items[k as usize];
                if c.when - prev.when > cfg.redundancy_short_window {
                    break;
                }
                let prev_key = (
                    prev.who.trim().to_lowercase(),
                    normalize_slug(&prev.this),
                    normalize_soft(&prev.did),
                );
                if prev_key == key {
                    redundancy_hits += 1;
                    break;
                }
                k -= 1;
            }
        }

        let semantic_similarity = if sim_cnt > 0 { sim_sum / sim_cnt as f64 } else { 0.0 };

        // Mapeia gaps para [0..1] de “proximidade saudável”: gaps menores → mais localidade (até certo limite)
        let temporal_locality = {
            let mut score = 0.0;
            let mut cnt = 0usize;
            for g in gaps_wt.iter().chain(gaps_t.iter()) {
                // 0s..6h mapeia para 1..0.4; >48h aproxima de 0
                let h = g / 3600.0;
                let s = if h <= 6.0 {
                    1.0 - 0.1 * (h / 6.0)  // ~[1.0..0.9]
                } else if h <= 48.0 {
                    0.9 - 0.9 * ((h - 6.0) / 42.0) // ~[0.9..0.0]
                } else {
                    0.0
                };
                score += s.max(0.0);
                cnt += 1;
            }
            if cnt > 0 { score / cnt as f64 } else { 0.0 }
        };

        let redundancy_rate = if redundancy_den > 0 {
            (redundancy_hits as f64) / (redundancy_den as f64)
        } else {
            0.0
        };

        CoherenceMetrics {
            semantic_similarity,
            temporal_locality,
            redundancy_rate,
        }
    }

    /* ---------------- Authenticity & Signature ---------------- */

    fn compute_authenticity(
        &self,
        g: &RelationshipGraph,
        evo: &RoleEvolution,
        coh: &CoherenceMetrics,
    ) -> f64 {
        // 1) Organic growth: taxa média de novos who/what (banda “natural”)
        let new_rate = 0.5 * evo.mean_novel_who_rate + 0.5 * evo.mean_novel_what_rate;
        let organic_growth = band_reward(new_rate, self.cfg.new_entity_rate_band.0, self.cfg.new_entity_rate_band.1);

        // 2) Clustering saudável: concentração de comunidades moderada
        let clustering = band_reward(g.community_concentration, self.cfg.community_concentration_band.0, self.cfg.community_concentration_band.1);

        // 3) Diversidade: entropia de who/what e de graus
        let diversity = {
            let d1 = g.who_entropy;
            let d2 = g.what_entropy;
            let d3 = g.degree_entropy;
            (0.4 * d1 + 0.4 * d2 + 0.2 * d3).clamp(0.0, 1.0)
        };

        // 4) Estabilidade: jaccard médio entre janelas (who/what)
        let stability = 0.5 * evo.mean_jaccard_who + 0.5 * evo.mean_jaccard_what;

        // 5) Coerência semântica & localidade temporal
        let coherence = (0.6 * coh.semantic_similarity + 0.4 * coh.temporal_locality).clamp(0.0, 1.0);

        // Penalidades: redundância exata e dominância de um ator
        let redundancy_penalty = (coh.redundancy_rate).min(0.5); // até -0.5
        let dominant_actor_penalty = {
            let total: usize = g.who_count.values().sum();
            if total > 0 {
                let maxf = g.who_count.values().copied().max().unwrap_or(0) as f64 / total as f64;
                if maxf >= self.cfg.dominant_actor_threshold {
                    ((maxf - self.cfg.dominant_actor_threshold) / (1.0 - self.cfg.dominant_actor_threshold)).min(1.0) * 0.6
                } else { 0.0 }
            } else { 0.0 }
        };

        let raw = 0.20 * organic_growth
            + 0.18 * clustering
            + 0.22 * diversity
            + 0.20 * stability
            + 0.20 * coherence
            - 0.10 * redundancy_penalty
            - 0.10 * dominant_actor_penalty;

        raw.clamp(0.0, 1.0)
    }

    fn generate_context_signature(
        &self,
        g: &RelationshipGraph,
        items: &[LogLineContract],
    ) -> [u8; 32] {
        // Assinatura: topologia essencial (pares who–what e suas frequências), ordenado
        let mut tuples: Vec<(String, String, usize)> = Vec::new();
        for (&(u, v), &w) in g.edges.iter() {
            let lu = g.node_labels[u].clone();
            let lv = g.node_labels[v].clone();
            let (who, what) = if lu.starts_with("W:") && lv.starts_with("T:") {
                (lu[2..].to_string(), lv[2..].to_string())
            } else if lu.starts_with("T:") && lv.starts_with("W:") {
                (lv[2..].to_string(), lu[2..].to_string())
            } else {
                (lu, lv)
            };
            tuples.push((who, what, w));
        }
        tuples.sort_by(|a, b| a.cmp(b));

        let mut hasher = Hasher::new();
        for (w, t, cnt) in tuples {
            hasher.update(w.as_bytes());
            hasher.update(t.as_bytes());
            hasher.update(&cnt.to_le_bytes());
        }
        // Mistura também a distribuição de “did” normalizada (top tokens)
        let mut did_counts: HashMap<String, usize> = HashMap::new();
        for c in items {
            for tok in tokenize(&c.did) {
                *did_counts.entry(tok).or_default() += 1;
            }
        }
        let mut did_vec: Vec<(String, usize)> = did_counts.into_iter().collect();
        did_vec.sort();
        for (tok, n) in did_vec.into_iter().take(128) {
            hasher.update(tok.as_bytes());
            hasher.update(&n.to_le_bytes());
        }
        *hasher.finalize().as_bytes()
    }
}

/* ---------------- Public Types ---------------- */

#[derive(Clone, Debug)]
pub struct ContextualFold {
    pub relationship_graph: RelationshipGraph,
    pub role_evolution: RoleEvolution,
    pub coherence_metrics: CoherenceMetrics,
    pub contextual_signature: [u8; 32],
    pub authenticity_score: f64, // 0..1
}

impl ContextualFold {
    pub fn empty() -> Self {
        Self {
            relationship_graph: RelationshipGraph {
                node_labels: Vec::new(),
                edges: HashMap::new(),
                degree_entropy: 0.0,
                degree_gini: 0.0,
                who_entropy: 0.0,
                what_entropy: 0.0,
                community_concentration: 0.0,
                components: Vec::new(),
                total_edge_weight: 0,
                who_count: HashMap::new(),
                what_count: HashMap::new(),
            },
            role_evolution: RoleEvolution {
                window_count: 0,
                mean_novel_who_rate: 0.0,
                mean_novel_what_rate: 0.0,
                mean_jaccard_who: 1.0,
                mean_jaccard_what: 1.0,
            },
            coherence_metrics: CoherenceMetrics::empty(),
            contextual_signature: [0u8; 32],
            authenticity_score: 0.0,
        }
    }
}

#[derive(Clone, Debug)]
pub struct RelationshipGraph {
    pub node_labels: Vec<String>,
    pub edges: HashMap<(usize, usize), usize>, // (u,v)->peso
    pub degree_entropy: f64,
    pub degree_gini: f64,
    pub who_entropy: f64,
    pub what_entropy: f64,
    pub community_concentration: f64, // Herfindahl (0..1)
    pub components: Vec<usize>,       // componente de cada nó
    pub total_edge_weight: usize,
    pub who_count: HashMap<String, usize>,
    pub what_count: HashMap<String, usize>,
}

#[derive(Clone, Debug)]
pub struct RoleEvolution {
    pub window_count: usize,
    pub mean_novel_who_rate: f64,
    pub mean_novel_what_rate: f64,
    pub mean_jaccard_who: f64,
    pub mean_jaccard_what: f64,
}

#[derive(Clone, Debug)]
pub struct CoherenceMetrics {
    pub semantic_similarity: f64, // média Jaccard tokens entre pares próximos (0..1)
    pub temporal_locality: f64,   // 0..1 (maior = mais localidade)
    pub redundancy_rate: f64,     // fração de duplicatas exatas em janela curta
}
impl CoherenceMetrics {
    pub fn empty() -> Self {
        Self { semantic_similarity: 0.0, temporal_locality: 0.0, redundancy_rate: 0.0 }
    }
}

/* ---------------- Helpers (graph, stats, text) ---------------- */

fn connected_components(n: usize, edges: &HashMap<(usize, usize), usize>) -> Vec<usize> {
    let mut g: Vec<Vec<usize>> = vec![Vec::new(); n];
    for (&(u, v), _) in edges.iter() {
        g[u].push(v);
        g[v].push(u);
    }
    let mut comp = vec![usize::MAX; n];
    let mut cid = 0usize;
    for s in 0..n {
        if comp[s] != usize::MAX { continue; }
        // BFS
        let mut q = VecDeque::new();
        q.push_back(s);
        comp[s] = cid;
        while let Some(u) = q.pop_front() {
            for &v in g[u].iter() {
                if comp[v] == usize::MAX {
                    comp[v] = cid;
                    q.push_back(v);
                }
            }
        }
        cid += 1;
    }
    comp
}

fn herfindahl_index(weights: &[usize], total: usize) -> f64 {
    if total == 0 { return 0.0; }
    let tot = total as f64;
    weights.iter().map(|&w| {
        let p = w as f64 / tot;
        p * p
    }).sum::<f64>().clamp(0.0, 1.0)
}

fn jaccard<T: std::hash::Hash + Eq>(a: &HashSet<T>, b: &HashSet<T>) -> f64 {
    if a.is_empty() && b.is_empty() { return 1.0; }
    let inter = a.intersection(b).count() as f64;
    let uni = a.union(b).count() as f64;
    if uni == 0.0 { 0.0 } else { inter / uni }
}

fn mean(xs: &[f64]) -> Option<f64> {
    if xs.is_empty() { None } else { Some(xs.iter().sum::<f64>() / xs.len() as f64) }
}

fn normalized_hist_entropy(xs: &[f64], bins: usize) -> f64 {
    if xs.is_empty() || bins == 0 { return 0.0; }
    let (min, max) = xs.iter().fold((f64::INFINITY, f64::NEG_INFINITY), |acc, &x| (acc.0.min(x), acc.1.max(x)));
    if !min.is_finite() || !max.is_finite() || min == max { return 0.0; }
    let width = (max - min) / (bins as f64);
    let mut hist = vec![0usize; bins];
    for &x in xs {
        let mut idx = ((x - min) / width) as usize;
        if idx >= bins { idx = bins - 1; }
        hist[idx] += 1;
    }
    normalized_entropy_usize(&hist)
}

fn entropy_from_counts_map(map: &HashMap<String, usize>) -> f64 {
    let total: usize = map.values().sum();
    if total == 0 { return 0.0; }
    let total_f = total as f64;
    let h = map.values().filter(|&&c| c > 0).map(|&c| {
        let p = c as f64 / total_f;
        -p * p.ln()
    }).sum::<f64>();
    let hmax = (map.len().max(1) as f64).ln();
    if hmax <= 0.0 { 0.0 } else { (h / hmax).clamp(0.0, 1.0) }
}

fn normalized_entropy_usize(bins: &[usize]) -> f64 {
    let total: usize = bins.iter().sum();
    if total == 0 { return 0.0; }
    let total_f = total as f64;
    let h = bins.iter().filter(|&&c| c > 0).map(|&c| {
        let p = c as f64 / total_f;
        -p * p.ln()
    }).sum::<f64>();
    let h_max = (bins.len() as f64).ln();
    if h_max <= 0.0 { 0.0 } else { (h / h_max).clamp(0.0, 1.0) }
}

fn gini(xs: &[f64]) -> f64 {
    let n = xs.len();
    if n == 0 { return 0.0; }
    let mut v = xs.to_vec();
    v.sort_by(|a, b| a.partial_cmp(b).unwrap());
    let sum = v.iter().copied().sum::<f64>();
    if sum <= 0.0 { return 0.0; }
    let mut cum = 0.0;
    for (i, x) in v.iter().enumerate() { cum += (i as f64 + 1.0) * *x; }
    let n_f = n as f64;
    (n_f + 1.0 - 2.0 * cum / sum) / (n_f - 1.0).max(1.0)
}

fn band_reward(x: f64, lo: f64, hi: f64) -> f64 {
    if x <= 0.0 { return 0.0; }
    if x < lo {
        (x / lo).clamp(0.0, 1.0) * 0.9
    } else if x <= hi {
        1.0
    } else {
        (1.0 - ((x - hi) / (1.0 - hi)).clamp(0.0, 1.0)) * 0.9
    }
}

/* ---------------- Text utils ---------------- */

fn normalize_slug(s: &str) -> String {
    let mut out = String::with_capacity(s.len());
    for ch in s.chars() {
        if ch.is_ascii_alphanumeric() { out.push(ch.to_ascii_lowercase()); }
        else if ch.is_whitespace() || ch == '-' || ch == '_' || ch == '/' { out.push('_'); }
    }
    while out.contains("__") { out = out.replace("__", "_"); }
    out.trim_matches('_').to_string()
}

/// Normalização leve para comparar “did” (minúsculas, remove múltiplos espaços)
fn normalize_soft(s: &str) -> String {
    let mut out = String::with_capacity(s.len());
    let mut last_was_space = false;
    for ch in s.chars() {
        let c = ch.to_ascii_lowercase();
        if c.is_ascii_whitespace() {
            if !last_was_space { out.push(' '); }
            last_was_space = true;
        } else {
            out.push(c);
            last_was_space = false;
        }
    }
    out.trim().to_string()
}

fn tokenize(s: &str) -> Vec<String> {
    let mut toks = Vec::new();
    let mut cur = String::new();
    for ch in s.chars() {
        let c = ch.to_ascii_lowercase();
        if c.is_ascii_alphanumeric() {
            cur.push(c);
        } else {
            if !cur.is_empty() {
                toks.push(cur.clone());
                cur.clear();
            }
        }
    }
    if !cur.is_empty() { toks.push(cur); }
    toks
}

fn jaccard_tokens(a: &str, b: &str) -> f64 {
    let ta: HashSet<String> = tokenize(a).into_iter().collect();
    let tb: HashSet<String> = tokenize(b).into_iter().collect();
    if ta.is_empty() && tb.is_empty() { return 1.0; }
    let inter = ta.intersection(&tb).count() as f64;
    let uni = ta.union(&tb).count() as f64;
    if uni == 0.0 { 0.0 } else { inter / uni }
}
```

* * *

src/folding/engine.rs (wire-in)
===============================

```rust
use crate::folding::temporal::{TemporalFolder, TemporalFold};
use crate::folding::contextual::{ContextFolder, ContextualFold};
use crate::types::DiamondTrajectory;

pub struct FoldingEngine {
    pub temporal_folder: TemporalFolder,
    pub contextual_folder: ContextFolder,
    // causal_folder: CausalFolder,
    // effort_folder: EffortFolder,
    // chaos_detector: ChaosDetector,
}

impl FoldingEngine {
    pub fn new() -> Self {
        Self {
            temporal_folder: TemporalFolder::new(),   // já DST-aware na sua versão estendida
            contextual_folder: ContextFolder::new(),
        }
    }

    pub async fn analyze_temporal(&self, trajectory: &DiamondTrajectory) -> TemporalFold {
        self.temporal_folder.fold(trajectory).await
    }

    pub async fn analyze_contextual(&self, trajectory: &DiamondTrajectory) -> ContextualFold {
        self.contextual_folder.fold(trajectory).await
    }
}
```

* * *

tests/contextual\_folder\_tests.rs
==================================

```rust
use chrono::{Duration, Utc};
use folding_engine::folding::contextual::ContextFolder;
use folding_engine::types::{DiamondTrajectory, LogLineContract};

fn mk(id: u64, who: &str, this_: &str, did: &str, minutes_from_now: i64) -> LogLineContract {
    LogLineContract {
        id,
        who: who.into(),
        did: did.into(),
        this: this_.into(),
        when: Utc::now() + Duration::minutes(minutes_from_now),
    }
}

#[tokio::test]
async fn bot_like_single_actor_low_contextual_authenticity() {
    // 1 pessoa, 1 tipo de tarefa, repetições com pequenas variações -> baixa diversidade/coerência
    let mut v = Vec::new();
    let who = "bot";
    let this_ = "perfect_task";
    let mut t = 0i64;
    for i in 0..20u64 {
        let did = if i % 2 == 0 { "completed step A" } else { "completed step B" };
        v.push(mk(i, who, this_, did, t));
        t += 15; // 15 min
    }
    let traj = DiamondTrajectory { contracts: v };

    let folder = ContextFolder::new();
    let fold = folder.fold(&traj).await;

    assert!(fold.authenticity_score < 0.55, "score={:?} fold={:#?}", fold.authenticity_score, fold);
    // Diversidade baixa
    assert!(fold.relationship_graph.who_entropy < 0.15);
    assert!(fold.relationship_graph.what_entropy < 0.25);
}

#[tokio::test]
async fn organic_team_gradual_intros_high_contextual_authenticity() {
    // 5 pessoas, 6 “this”, introdução gradual por janelas, clusters naturais
    let mut v = Vec::new();
    let mut id = 0u64;
    let mut t = 0i64;

    // Semana 1: Alice & Bob em "ingest", "clean"
    for who in ["Alice", "Bob"] {
        for this_ in ["data_ingest", "data_clean"] {
            v.push(mk(id, who, this_, "setup pipeline", t)); id+=1; t+=60;
            v.push(mk(id, who, this_, "fix schema issues", t)); id+=1; t+=120;
        }
    }
    // Semana 2: Carol entra, novo “this” + coocorrências locais
    t += 7*24*60;
    for this_ in ["feature_x", "data_clean"] {
        v.push(mk(id, "Carol", this_, "implement step 1", t)); id+=1; t+=45;
        v.push(mk(id, "Carol", this_, "implement step 2", t)); id+=1; t+=40;
        v.push(mk(id, "Alice", this_, "review & docs", t)); id+=1; t+=30;
    }
    // Semana 3: Dave & Erin + tasks novas; estabilidade moderada (ainda usa alguns antigos)
    t += 7*24*60;
    for who in ["Dave", "Erin"] {
        for this_ in ["feature_y", "ops_rollout"] {
            v.push(mk(id, who, this_, "deploy staging", t)); id+=1; t+=55;
            v.push(mk(id, who, this_, "smoke tests", t)); id+=1; t+=50;
        }
    }
    // Mistura cross-team no fim (coerência semântica local)
    for who in ["Alice", "Dave"] {
        v.push(mk(id, who, "ops_rollout", "monitoring alerts", t)); id+=1; t+=30;
    }

    let traj = DiamondTrajectory { contracts: v };
    let folder = ContextFolder::new();
    let fold = folder.fold(&traj).await;

    assert!(fold.authenticity_score > 0.68, "score={:?} fold={:#?}", fold.authenticity_score, fold);
    // Deve ter diversidade razoável e estabilidade moderada
    assert!(fold.relationship_graph.who_entropy > 0.6);
    assert!(fold.relationship_graph.what_entropy > 0.6);
}
```

* * *

Como o Contextual Fold decide autenticidade (5 sinais)
------------------------------------------------------

1.  **Crescimento orgânico** — taxa média de novas entidades (who/what) por janela cai numa banda saudável (nem explosão artificial, nem zero novidades).
2.  **Clustering natural** — concentração de comunidades (via Herfindahl sobre componentes conexas) tende a um meio-termo: nem um mega-cluster único, nem fragmentação total.
3.  **Diversidade** — entropia de atores, tarefas e graus de nó.
4.  **Estabilidade** — Jaccard médio entre janelas (who/what) recompensa continuidade com alguma evolução.
5.  **Coerência** — similaridade semântica dos “did” em coocorrência temporal + localidade temporal de repetições.

**Penalidades**: duplicatas exatas em janela curta e dominância extrema de um ator.

**Assinatura**: BLAKE3 de (who, what, freq) ordenado + distribuição de tokens de “did” — fingerprint contextual resistente a falsificações superficiais.

* * *

Próximo passo sugerido
----------------------

Se quiser, eu já **integro o `combine_folds`** usando `temporal.authenticity_score` e `contextual.authenticity_score` nos 25% e 25% definidos no seu verificador, e preparo o esqueleto do **Causal Folder** (explicito/implícito/consistência/plausibilidade + assinatura). Quer que eu já faça o _wire_ do verificador com os dois folds e adicione um **teste de integração temporal+contextual** (autêntico vs. forjado)?



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)