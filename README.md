# NAME

XML::Compile::SOAP::Daemon::Dancer2 - simple implementation of a WSDL server within Dancer2

# SYNOPSIS

    package MyDancer2App;
    use Dancer2;
    use XML::Compile::SOAP::Daemon::Dancer2;

    wsdl_endpoint '/calculator', {
        wsdl                    => 'calculator.wsdl',
        xsd                     => [],
        implementation_class    => 'Calculator',
        operations  => {
            add => sub {
                my ( $soap, $data, $dsl ) = @_;
                $dsl->error( $dsl->to_dumper( $data ) );
                return +{
                    Result => $data->{parameters}->{x} + $data->{parameters}->{y},
                };
            },
        }
    };

# DESCRIPTION

XML::Compile::SOAP::Daemon::Dancer2 is a plugin to add a SOAP endpoint to a Dancer2 app

The plugin is Heavily inspired by XML::Compile::SOAP::Daemon::PSGI

The plugin export a keyword wsdl\_endpoint, that takes 2 arguments, a route path and an options hashref.

Options available are:
\* wsdl: name of the wsdl file (under appdir/wsdl)
\* xsd: an arrayref of xsd file (under appdir/wsdl)
\* implementation\_class: name of class to implement the operations
\* operations: hashref with soap operation name as key, sub as value
\* implementation\_class : the class needs to do the role XML::Compile::SOAP::Daemon::Dancer2::Role::Implementation

For each operation, define a sub in the implementation class, soapaction\_{operation\_name}, each sub will be called with
the following parameters: $soap, $data, $dsl

operations: each sub will be called with parameters: $soap, $data, $dsl

# AUTHOR

Pierre VIGIER <pierre.vigier@gmail.com>

# COPYRIGHT

Copyright 2016- Pierre VIGIER

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
