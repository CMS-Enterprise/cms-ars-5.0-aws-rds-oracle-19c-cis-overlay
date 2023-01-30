# cms-ars-5.0-aws-rds-oracle-19c-cis-overlay

InSpec profile overlay to validate the secure configuration of an AWS RDS instance of Oracle Database 19c against the Oracle Database 19c Benchmark version 1.0.0 [CIS](https://www.cisecurity.org/cis-benchmarks/)
tailored for CMS ARS 5.0.

## Getting Started  
### InSpec (CINC-auditor) setup
For maximum flexibility/accessibility, we’re moving to “cinc-auditor”, the open-source packaged binary version of Chef InSpec, compiled by the CINC (CINC Is Not Chef) project in coordination with Chef using Chef’s always-open-source InSpec source code. For more information: https://cinc.sh/

It is intended and recommended that CINC-auditor and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target. This can be any Unix/Linux/MacOS or Windows runner host, with access to the Internet.

__For the best security of the runner, always install on the runner the _latest version_ of CINC-auditor.__ 

__The simplest way to install CINC-auditor is to use this command for a UNIX/Linux/MacOS runner platform:__
```
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-auditor
```

__or this command for Windows runner platform (Powershell):__
```
. { iwr -useb https://omnitruck.cinc.sh/install.ps1 } | iex; install -project cinc-auditor
```
To confirm successful install of cinc-auditor:
```
cinc-auditor -v
```
> sample output:  _4.24.32_

Latest versions and other installation options are available at https://cinc.sh/start/auditor/.

### Requirements
- AWS RDS instance of the Oracle 19c Database
- An account with at least SYSTEM-level role access to run SQL commands
- [SQL\*Plus](https://docs.oracle.com/cd/B19306_01/server.102/b14357/qstart.htm) - Release 19c on the runner

## Specify your BASELINE system categization as an environment variable:
### (if undefined defaults to Moderate baseline)

```
# BASELINE (choices: Low, Low-HVA, Moderate, Moderate-HVA, High, High-HVA)
# (if undefined defaults to Moderate baseline)

# on linux:
BASELINE=High

# on Powershell:
$env:BASELINE="High"
```

## Inputs: Tailoring your scan to Your Environment

The following inputs must be configured in an inputs ".yml" file for the profile to run correctly for your specific environment. More information about InSpec inputs can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).

### Example Inputs You Can Use

```yaml

# description: Username Oracle DB
user: 'admin'

# description: Password Oracle DB
password: 'xvIA7zonxGM=1'

# description: Hostname Oracle DB
host: '<AWS_RDS_Endpoint>.rds.amazonaws.com'

# description: Service name Oracle DB
service: 'ORCL'

# description: Location of sqlplus tool (try `which sqlplus`)
sqlplus_bin: '/opt/oracle/product/12.2.0.1/dbhome_1/bin/sqlplus'

# Description: 'Flag to indicate whether the database is multi-tenant or single-tenant'
multitenant: false

# Description: 'Expected version for Oracle database'
version: '19.0.0.0.0'

```
##### Note
Environment variables will not be interpreted correctly in `inputs.yml` or `inspec.yml`.
Example:
```
sqlplus_bin: $ORACLE_HOME/bin/sqlplus # $ORACLE_HOME will not be expanded out correctly!
```

# Running This Baseline Directly from Github

```
# How to run
cinc-auditor exec https://github.com/CMS-Enterprise/cms-ars-5.0-aws-rds-oracle-19c-cis-overlay/archive/main.tar.gz --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter=cli json:<path_to_store_your_output_file/name_of_your_output_file.json>
```

### Different Run Options

  [Full exec options](https://docs.chef.io/inspec/cli/#options-3)

## Running This Baseline from a local Archive copy 

If your runner is not always expected to have direct access to GitHub, use the following steps to create an archive bundle of this baseline and all of its dependent tests:

(Git is required to clone the InSpec profile using the instructions below. Git can be downloaded from the [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) site.)

When the __"runner"__ host uses this profile baseline for the first time, follow these steps: 

```
mkdir profiles
cd profiles
git clone https://github.com/CMS-Enterprise/cms-ars-5.0-aws-rds-oracle-19c-cis-overlay
cinc-auditor archive cms-ars-5.0-aws-rds-oracle-19c-cis-overlay
cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter=cli json:<path_to_store_your_output_file/name_of_your_output_file.json>
```
For every successive run, follow these steps to always have the latest version of this baseline:

```
cd cms-ars-5.0-aws-rds-oracle-19c-cis-overlay
git pull
cd ..
cinc-auditor archive cms-ars-5.0-aws-rds-oracle-19c-cis-overlay --overwrite
cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter=cli json:<path_to_store_your_output_file/name_of_your_output_file.json>
```

## Using Heimdall for Viewing the JSON Results

The JSON results output file can be loaded into __[heimdall-lite](https://heimdall-lite.cms.gov/)__ for a user-interactive, graphical view of the InSpec results. 

The JSON InSpec results file may also be loaded into a __[full heimdall server](https://github.com/mitre/heimdall2)__, allowing for additional functionality such as to store and compare multiple profile runs.

## Authors
* Eugene Aronne - [ejaronne](https://github.com/ejaronne)

## Special Thanks
* Aaron Lippold - [aaronlippold](https://github.com/aaronlippold)
* Shivani Karikar - [karikarshivani](https://github.com/karikarshivani)

## Contributing and Getting Help
To report a bug or feature request, please open an [issue](https://github.com/cms-enterprise/cms-ars-5.0-aws-rds-oracle-19c-cis-overlay/issues/new).

### NOTICE

© 2018-2022 The MITRE Corporation.

Approved for Public Release; Distribution Unlimited. Case Number 18-3678.

### NOTICE 

MITRE hereby grants express written permission to use, reproduce, distribute, modify, and otherwise leverage this software to the extent permitted by the licensed terms provided in the LICENSE.md file included with this project.

### NOTICE  

This software was produced for the U. S. Government under Contract Number HHSM-500-2012-00008I, and is subject to Federal Acquisition Regulation Clause 52.227-14, Rights in Data-General.  

No other use other than that granted to the U. S. Government, or to those acting on behalf of the U. S. Government under that Clause is authorized without the express written permission of The MITRE Corporation.

For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA  22102-7539, (703) 983-6000.

### NOTICE 

CIS Benchmarks are published by the Center for Internet Security (CIS), see: https://www.cisecurity.org/cis-benchmarks/.
