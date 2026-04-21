# Drill Down

## Table of Contents
- [Overview](#overview)
- [Enabling Drill-Down](#enabling-drill-down)
- [Breadcrumb Navigation](#breadcrumb-navigation)
- [Custom Drill Paths](#custom-drill-paths)
- [Reset and State Management](#reset-and-state-management)

## Overview
Patterns for hierarchical navigation inside a TreeMap, including `EnableDrillDown`, `DrillDownView`, `EnableBreadcrumb`, `BreadcrumbConnector`, and `TreeMapInitialDrillSettings`.

## Enabling Drill-Down
- Handle `OnDrillStart` and `DrillCompleted` events, or enable drill down directly on the TreeMap.

## Breadcrumb Navigation
- Maintain breadcrumb state to allow users to navigate back through levels.

## Custom Drill Paths
- Define custom paths or filters when the default grouping does not match UX needs.

## Reset and State Management
- Provide a reset action to return to the root level and preserve selection state as required.
