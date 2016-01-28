# Nagios Reader to Centreon CLAPI

"Nagios Reader to Centreon CLAPI" is a free and open source project to analyse
Nagios CFG configuration files and to transform monitoring configuration to
Centreon CLAPI command in order to import configuration into Centreon web
interface.

## Prerequisites

First of all you need a CES server installed and ready to use. Please see the
document on document.centreon.com to install a Centreon server based on CES.

## Installation

This script uses the Perl-Nagios-Object library to read CFG files. To install
it please follow this steps on your Nagios(R) server:

    $ yum install perl-Module-Build perl-Test-Exception perl-Test-NoWarnings perl-List-Compare
	$ cd /tmp
	$ wget http://search.cpan.org/CPAN/authors/id/D/DU/DUNCS/Nagios-Object-0.21.20.tar.gz
	$ tar xzf Nagios-Object-0.21.20.tar.gz
	$ cd Nagios-Object-0.21.20
	$ perl Build.PL
    $ ./Build
    $ ./Build test
    $ ./Build install

Note : perl-List-Compare is from EPEL repo for CentOS/Red Hat

## Usage

On a fresh CES server the default poller is named "Central". If you rename it
or if you want to link this Nagios configuration to a predifined poller you 
have to change the poller name on line 65

    my $default_poller = "Central";

To display help use the command:

    $ perl nagios_reader_to_centreon_clapi.pl --help
    ######################################################
    #    Copyright (c) 2005-2016 Centreon                #
    #    Bugs to http://github.com/nagiosToCentreon      #
    ######################################################
    
    Usage: nagios_reader_to_centreon_clapi.pl
        -V (--version)     Show script version
        -h (--help)        Usage help
        -C (--config)      Path to nagios.cfg file
		-S (--servcies-hg) To keep services by hostgroups definition

Note : By default services by hostgroups will be transformed. A template of 
service will be created using old service definition and a unitary service by 
host (for all hosts linked to the hostgroup) will be created linked to the 
previous service template.

To run the script please use the following command:

    $ perl nagios_reader_to_centreon_clapi.pl --config /usr/local/nagios/etc/nagios.cfg > /tmp/centreon_clapi_import_commands.txt

Export the file /tmp/centreon_clapi_import_commands.txt on your CES server.

Run the following command to import configuration into Centreon on your CES server:

    $ /usr/share/centreon/www/modules/centreon-clapi/core/centreon -u admin -p @PASSWORD -i /tmp/centreon_clapi_import_commands.txt

