# Initial Reading Notes - CaraServe

## First Impressions
- 

## Key Questions to Investigate
1. How exactly does the CPU-GPU synchronization work?
2. What are the specific performance bottlenecks in current LoRA serving?
3. How does the rank-aware scheduling algorithm decide priorities?

## Technical Details to Understand
- [ ] Base model management on GPU
- [ ] LoRA adapter loading mechanism
- [ ] CPU prefilling process
- [ ] GPU handoff protocol
- [ ] Scheduling algorithm details

## Implementation Plan Thoughts
- Start with understanding the LightLLM codebase
- Identify the core CaraServe components
- Build simplified version first
- Add complexity incrementally

## Questions for the Paper
- What happens if CPU computation can't keep up?
- How does memory usage scale with number of adapters?
- What are the trade-offs of different rank sizes?

---
*Notes taken on: [Date]* 