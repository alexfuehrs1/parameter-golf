# Skill: Artifact Budget Discipline

## Purpose
Treat artifact size as a first-class optimization target.

## Instructions

Whenever architecture, serialization, quantization, packaging, or code layout changes:
- estimate impact on total artifact bytes
- separate code bytes from model / compressed weight bytes
- identify growth risks early
- prefer approaches that preserve artifact headroom

## Rules

- Never discuss a model change without discussing its artifact impact
- Never add support code casually if bytes are tight
- Prefer compact serialization and compact implementation paths
- Track both raw checkpoint size and final submitted artifact size when relevant

## Checklist

- parameter count implication
- quantization implication
- serialization implication
- compression implication
- code footprint implication
- safety margin to budget cap

## Output

### Current Budget Estimate
...

### Change Impact
...

### Budget Risk
...

### Mitigation Options
...