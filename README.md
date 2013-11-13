[![Build Status](https://travis-ci.org/moznion/Plack-Request-WithEncoding.png?branch=master)](https://travis-ci.org/moznion/Plack-Request-WithEncoding) [![Coverage Status](https://coveralls.io/repos/moznion/Plack-Request-WithEncoding/badge.png?branch=master)](https://coveralls.io/r/moznion/Plack-Request-WithEncoding?branch=master)
# NAME

Plack::Request::WithEncoding - Subclass of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request) which supports encoding.

# SYNOPSIS

    use Plack::Request::WithEncoding;

    my $app_or_middleware = sub {
        my $env = shift; # PSGI env

        # Example of $env
        #
        # $env = {
        #     QUERY_STRING   => 'query=%82%d9%82%b0', # <= encoded by 'cp932'
        #     REQUEST_METHOD => 'GET',
        #     HTTP_HOST      => 'example.com',
        #     PATH_INFO      => '/foo/bar',
        # };

        my $req = Plack::Request::WithEncoding->new($env);

        $req->env->{'plack.request.withencoding.encoding'} = 'cp932'; # <= specify the encoding method.

        # If `$req->env->{'plack.request.withencoding.encoding'}` is undef (namely, the encoding method is not specified),
        # then this library will use `utf-8` as encoding method.

        my $query = $req->param('query'); # <= get parameters of 'query' that is decoded by 'cp932'.

        my $res = $req->new_response(200); # new Plack::Response
        $res->finalize;
    };

# DESCRIPTION

Plack::Request::WithEncoding is the subclass of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request).
This module supports the encoding for requests, the following attributes will return decoded request values.

# ATTRIBUTES

- encoding

    Returns a encoding method to use to decode parameters.

- query\_parameters

    Returns a reference to a hash containing __decoded__ query string (GET)
    parameters. This hash reference is [Hash::MultiValue](http://search.cpan.org/perldoc?Hash::MultiValue) object.

- body\_parameters

    Returns a reference to a hash containing __decoded__ posted parameters in the
    request body (POST). As with `query_parameters`, the hash
    reference is a [Hash::MultiValue](http://search.cpan.org/perldoc?Hash::MultiValue) object.

- parameters

    Returns a [Hash::MultiValue](http://search.cpan.org/perldoc?Hash::MultiValue) hash reference containing __decoded__ (and merged) GET
    and POST parameters.

- param

    Returns __decoded__ GET and POST parameters with a CGI.pm-compatible param
    method. This is an alternative method for accessing parameters in
    $req->parameters. Unlike CGI.pm, it does _not_ allow
    setting or modifying query parameters.

        $value  = $req->param( 'foo' );
        @values = $req->param( 'foo' );
        @params = $req->param;

- raw\_query\_parameters

    This attribute is the same as `query_parameters` of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request).

- raw\_body\_parameters

    This attribute is the same as `body_parameters` of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request).

- raw\_parameters

    This attribute is the same as `parameters` of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request).

- raw\_param

    This attribute is the same as `param` of [Plack::Request](http://search.cpan.org/perldoc?Plack::Request).

# REQUEST ENVIRONMENTS of Plack

You can specify the encoding method, like so;

    $req->env->{'plack.request.withencoding.encoding'} = 'utf-7'; # <= set utf-7

And this encoding method will be used to decode.

Default encoding method is __utf-8__. If `$req->env->{'plack.request.withencoding.encoding'}` is undef
then default encoding method will be used.

# SEE ALSO

[Plack::Request](http://search.cpan.org/perldoc?Plack::Request)

# LICENSE

Copyright (C) moznion.

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# AUTHOR

moznion <moznion@gmail.com>
