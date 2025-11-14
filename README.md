# AI Workshop - OpenEdge ABL Project

This repository contains an OpenEdge ABL project demonstrating the Business Entity architectural pattern.

## Project Structure

- `src/` - Source code
  - `business/` - Business logic layer (Entity classes, datasets)
  - `*.w` - UI window files
- `dump/` - Database schema
- `doc/` - Documentation
- `.windsurf/` - Windsurf AI assistant configuration

## Business Entity Pattern

This project implements a 3-tier architecture:
1. **UI Layer** - Window files (.w)
2. **Business Logic Layer** - Entity classes (.cls)
3. **Data Abstraction Layer** - Dataset definitions (.i)

See `doc/business-entity-pattern.md` for detailed documentation.

## Database

Uses the Sports2000 database schema. See `dump/sports2000.df` for the complete schema definition.
