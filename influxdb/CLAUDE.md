# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is the InfluxDB documentation for Docker Hub Official Images, part of the docker-library/docs repository. The documentation covers all InfluxDB versions (3 Core, 3 Enterprise, v2, v1, and Enterprise v1) and generates the README.md that appears on Docker Hub.

## Key Architecture

### Template System
- **DO NOT EDIT `README.md`** - it's auto-generated from other files
- Edit `content.md` instead, which contains template placeholders:
  - `%%LOGO%%` - Logo placement
  - `%%COMPOSE%%` - Docker Compose file inclusion
  - `%%IMAGE%%` - Image name substitution
- The build system combines `content.md` with other component files to generate `README.md`

### File Structure
- `content.md` - Main documentation content (edit this)
- `compose.yaml` - Docker Compose examples for InfluxDB 3
- `variant-data.md` / `variant-meta.md` - Enterprise cluster documentation
- `get-help.md` - Support resources
- `README-short.txt` - Brief description (100 char limit)
- `metadata.json` - Docker Hub categories and metadata

## Development Commands

### Documentation Generation
```bash
# Generate README.md from component files
./update.sh influxdb

# Generate all documentation (parallel)
./parallel-update.sh

# Preview changes before committing
./update.sh influxdb && cat influxdb/README.md
```

### Validation and Formatting
```bash
# Check markdown formatting
./markdownfmt.sh -l influxdb          # List issues
./markdownfmt.sh -d influxdb          # Show diff
./markdownfmt.sh -w influxdb          # Fix formatting

# Check YAML formatting
./ymlfmt.sh influxdb/compose.yaml

# Validate metadata
./metadata.sh influxdb               # Check format and categories
./metadata.sh -w influxdb            # Apply fixes
```

### Development Workflow
1. Edit `content.md` or other component files (never `README.md`)
2. Run `./update.sh influxdb` to generate README.md
3. Run `./markdownfmt.sh -l influxdb` to check formatting
4. Run `./metadata.sh influxdb` to validate metadata
5. Commit changes (exclude generated `README.md`)

## Content Guidelines

- Keep descriptions concise and focused on the minimum necessary information for users to get started with InfluxDB.
- Include links to official documentation for detailed instructions.

### Multi-Version Support

The documentation covers:
- **InfluxDB 3 Core**: `influxdb:3-core` (latest, emphasized)
- **InfluxDB 3 Enterprise**: `influxdb:3-enterprise` (latest Enterprise)
- **InfluxDB v2**: `influxdb:2` (legacy)
- **InfluxDB v1**: `influxdb:1.11` (legacy)
- **InfluxDB Enterprise v1**: `influxdb:1.11-data`, `influxdb:1.11-meta` (legacy)

### Docker Command Standards
- Use backslashes (`\`) for line continuations in multi-line commands
- Use long-form command-line options--for example: `--publish` (not `-p`)
- Include working Docker Compose examples for each version
- Include working Docker CLI examples for each version
- Format commands to fit within 80 characters when possible

### Key Requirements
- **LICENSE**: InfluxDB 3 Enterprise requires license configuration
- **PORTS**: Core uses 8181, Enterprise uses 8181, v2 uses 8086
- **STORAGE**: Use proper volume mounts for data persistence
- **CLUSTERING**: Enterprise v1 requires meta and data node coordination. InfluxDB 3 Enterprise supports clustering with specific modes (compaction, read, process, etc.)

## Dependencies

### Required Tools
- `bashbrew` - For scraping Docker Library manifests and tag information
- `docker` - For running formatting/validation tools
- Standard Unix tools: `bash`, `sed`, `grep`

### Docker Images Used
- `tianon/markdownfmt` - Markdown formatting validation
- `tianon/ymlfmt` - YAML formatting validation

## Git Configuration

This is a fork of `docker-library/docs`:
- **Origin**: `git@github.com:jstirnaman/dockerhub-docs.git`
- **Upstream**: `git@github.com:docker-library/docs.git`

## Important Notes

- The repository follows Docker Official Images standards
- All changes must pass automated CI validation
- Documentation is deployed automatically to Docker Hub
- Template placeholders must be preserved during editing
- Metadata categories are limited to 3 maximum (currently: data-science, databases-and-storage, internet-of-things)