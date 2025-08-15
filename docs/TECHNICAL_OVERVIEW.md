# 🏗️ Technical Architecture Overview

> System design and data flow for the Advanced Insider Threat Dataset Generator

📖 **Navigation**: [← Main README](../README.md) | [Dataset Schema](DATA_SCHEMA.md) | [User Guide →](USER_GUIDE.md)

---

## 🔄 Data Generation Flow

```
Employee Profiles → Classification Assignment → Behavioral Simulation → Security Analysis → Export
     ↓                       ↓                        ↓                    ↓             ↓
  [Realistic            [6 Behavioral        [Daily           [Security  [CSV/Excel
   Employees]            Groups]              Activities]      Analysis]   + Reports]
```

### 🔐 **Classification-Aware Pipeline**
Each step incorporates security clearance levels and classification handling:
- **Employee Profiles** include security clearance levels (1-4)
- **Activity Generation** respects clearance-based access restrictions
- **Document Operations** track classification levels of handled materials
- **Risk Assessment** evaluates classification violations and unauthorized access

## 🎯 Core Components

### 1. Employee Generation Layer
**Location**: [`employee_generator/`](../src/employee_generator/)
- **Purpose**: Creates realistic employee profiles with departments, roles, and characteristics
- **Key Files**: 
  - [`employee_manager.py`](../src/employee_generator/employee_manager.py) - Manages employee collections
  - [`employee_profile_creator.py`](../src/employee_generator/employee_profile_creator.py) - Individual profile creation
- **Output**: Employee profiles with behavioral group assignments

### 2. Activity Simulation Layer
**Location**: [`activity_generators/`](../src/activity_generators/) + [`data_generator/`](../src/data_generator/)
- **Purpose**: Generates daily employee activities based on behavioral patterns
- **Activity Types**:
  - **Building Access** - Entry/exit patterns, multi-campus access
  - **Printing** - Document printing with volume and timing
  - **Document Burning** - Secure data destruction activities
  - **Travel** - Business trips and location tracking
- **Key Files**: 
  - [`data_generator.py`](../src/data_generator/data_generator.py) - Main generation engine
  - [`data_generator_core.py`](../src/data_generator/data_generator_core.py) - Core orchestrator that combines all activity generators
  - [`access_activity_generator.py`](../src/activity_generators/access_activity_generator.py) - Building access
  - [`print_activity_generator.py`](../src/activity_generators/print_activity_generator.py) - Printing behavior
  - [`burn_activity_generator.py`](../src/activity_generators/burn_activity_generator.py) - Document destruction
  - [`travel_activity_generator.py`](../src/activity_generators/travel_activity_generator.py) - Travel patterns

### 3. Analysis Layer
**Location**: [`analyzers/`](../src/analyzers/)
- **Purpose**: Analyzes generated data for behavioral patterns and security insights
- **Analysis Types**:
  - **Behavioral Analysis** - Group-specific patterns and comparisons
  - **Security Analysis** - Threat detection and risk assessment
  - **Statistical Analysis** - Data quality and trend analysis
- **Key Files**:
  - [`comprehensive_analyzer.py`](../src/analyzers/comprehensive_analyzer.py) - Complete analysis orchestration
  - [`behavioral_analyzer.py`](../src/analyzers/behavioral_analyzer.py) - Behavioral pattern analysis
  - [`security_analyzer.py`](../src/analyzers/security_analyzer.py) - Security threat analysis

### 4. Export and Documentation Layer
**Location**: [`data_exporter/`](../src/data_exporter/)
- **Purpose**: Exports datasets with comprehensive documentation and reports
- **Features**:
  - Multi-format export (CSV, Excel)
  - Automated data dictionaries
  - Analysis reports and summaries
- **Key Files**:
  - [`data_exporter_base.py`](../src/data_exporter/data_exporter_base.py) - Main export functionality
  - [`data_dictionary_generator.py`](../src/data_exporter/data_dictionary_generator.py) - Documentation creation
  - [`report_generator.py`](../src/data_exporter/report_generator.py) - Analysis reporting

## ⚙️ Configuration System

**Location**: [`config/`](../src/config/)

The system uses a hierarchical configuration approach:

```
Config (Unified) ← behavioral_patterns.py (6 Groups)
    ↑           ← organizational_structure.py (11 Departments)  
    ↑           ← employee_attributes.py (Security & Demographics)
    ↑           ← geographic_data.py (Countries & Locations)
```

**Key Configuration Files**:
- [`config.py`](../src/config/config.py) - Unified configuration class
- [`behavioral_patterns.py`](../src/config/behavioral_patterns.py) - Employee behavioral groups A-F
- [`organizational_structure.py`](../src/config/organizational_structure.py) - Departments and positions
- [`employee_attributes.py`](../src/config/employee_attributes.py) - Security classifications and demographics
- [`geographic_data.py`](../src/config/geographic_data.py) - Campus locations and travel data

## 🔧 Supporting Infrastructure

### Core Services
**Location**: [`core/`](../src/core/)
- [`workflow_manager.py`](../src/core/workflow_manager.py) - Orchestrates generation and analysis workflows
- [`config_manager.py`](../src/core/config_manager.py) - System setup and logging configuration
- [`daily_label_creator.py`](../src/core/daily_label_creator.py) - Transforms employee-level to daily-level labels
- [`data_noise_injector.py`](../src/core/data_noise_injector.py) - Adds synthetic noise for realism

### Command-Line Interface
**Location**: [`cli/`](../src/cli/)
- [`argument_parser.py`](../src/cli/argument_parser.py) - Command-line argument parsing and validation
- [`display_utils.py`](../src/cli/display_utils.py) - User interface and output formatting

### Utilities
**Location**: [`utils/`](../src/utils/)
- [`constants.py`](../src/utils/constants.py) - System-wide constants and configuration values

## 📈 Data Pipeline

1. **Configuration Loading** → System loads behavioral patterns and organizational structure
2. **Employee Generation** → Creates employee profiles with realistic characteristics
3. **Daily Simulation** → Generates activities for each employee over specified time period
4. **Pattern Application** → Applies behavioral group patterns to activities
5. **Risk Assessment** → Calculates risk indicators and security metrics
6. **Quality Enhancement** → Optional noise injection for enhanced realism
7. **Analysis** → Comprehensive behavioral and security analysis
8. **Export** → Multi-format output with documentation and reports

## 🎯 Behavioral Groups

The system models six distinct behavioral groups:

- **Group A**: Executive Management - Irregular hours, moderate printing, high travel
- **Group B**: Developers & Engineers - Technical staff, some late hours, low travel
- **Group C**: Office Workers - Regular hours, high printing activity, low travel
- **Group D**: Marketing & Business - Regular hours, moderate travel, standard printing
- **Group E**: Security Personnel - 24/7 shifts, high security access, specialized patterns
- **Group F**: IT Staff - Technical roles, irregular hours, high burning activity

## 📚 Detailed Documentation

- **[Main Project Overview](../README.md)** - Project introduction and quick start
- **[CLI Usage Guide](../src/cli/README.md)** - Complete command-line reference
- **[Configuration Guide](../src/config/README.md)** - Behavioral patterns and customization
- **[Activity Generation](../src/activity_generators/README.md)** - Activity simulation details
- **[Analysis Features](../src/analyzers/README.md)** - Analysis capabilities and metrics
- **[Export Documentation](../src/data_exporter/README.md)** - Export formats and reports
- **[Core Infrastructure](../src/core/README.md)** - System architecture and utilities

## 🤝 Getting Started

1. **Installation** - Clone repository and install dependencies
2. **Basic Generation** - Run `python main.py` with default settings
3. **Explore Output** - Review generated CSV/Excel files and analysis reports
4. **Customize** - Modify configuration files for specific research needs
5. **Analyze** - Use built-in analyzers or export data for external analysis

**Next Steps**: Check the [CLI Usage Guide](../src/cli/README.md) for detailed examples and options.
