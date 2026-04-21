# Troubleshooting

## Table of Contents
- [Overview](#overview)
- [Rendering Problems](#rendering-problems)
- [Data Binding Issues](#data-binding-issues)
- [Performance Issues](#performance-issues)
- [Common Fixes](#common-fixes)

## Overview
Quick list of common problems and how to resolve them.

## Rendering Problems
- Verify `DataSource`, `GroupPath`, and layout settings. Ensure CSS does not hide elements.

## Data Binding Issues
- Check for null/undefined values, inconsistent nesting, or incorrect `WeightValuePath`.

## Performance Issues
- Reduce item count, aggregate data, or use virtualized strategies; avoid expensive templates on thousands of items.

## Common Fixes
- Restore default settings to verify behavior, log data shapes before binding, and progressively enable features to identify regressions.
