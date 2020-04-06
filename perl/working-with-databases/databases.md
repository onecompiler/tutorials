Perl DBI(Database Independent Interface for Perl) module helps to interact with many of databases including Oracle, SQL Server, MySQL, Sybase, etc.

In this chapter, let's learn how to interact with MySQL database in Perl.

## How to install DBD::mysql Module

* Install MySQL driver to connect to MySQL databases(assuming you have MySQL server installed on your machine.)

There are multiple ways to install MySQL drivers, one of the ways is by using `CPAN`.

  ```sh
  perl -MCPAN -e shell
  ```
* To install `DBD::mysql` use the below command,

  ```sh
  install DBD:mysql
  ```
* Installation progress can be seen in command prompt.

## How to connect to MySQL database server

Consider we are creating  a sample database called `test`

  ```sh
  my $dbh = DBI->connect("DBI:mysql:test",'root','');
  ```

DBI->connect has three parameters:
1. Data  source name
2. Username
3. Password

DBI->connect returns false when the connection is not established.

## Insert data in to the database

Once the connection is established, you can insert data into the tables. Consider you are inserting data into DATA_TABLE
  ```perl
  my $sth = $dbh->prepare("INSERT INTO DATA_TABLE
                        (NAME, ID, OCCUPATION )
                          values
                        ('foo', 123, 'doctor')");
  $sth->execute() or die $DBI::errstr;
  print "Inserted rows:" + $sth->rows;
  $sth->finish();
  $dbh->commit or die $DBI::errstr;
  ```

## Read data from tables

  ```perl
  my $sth = $dbh->prepare("SELECT NAME, ID
                          FROM DATA_TABLE 
                          WHERE OCCUPATION = 'doctor'");
  $sth->execute() or die $DBI::errstr;
  print "Number of records whose occupation is doctor :" + $sth->rows;
  while (my @row = $sth->fetchrow_array()) {
    my ($name, $id ) = @row;
    print "Name = $name, ID = $id\n";
  }
  $sth->finish();
  ```

## Updating a record

  ```perl
  my $sth = $dbh->prepare("UPDATE DATA_TABLE
                          SET OCCUPATION = 'student' 
                          WHERE ID = 123");
  $sth->execute() or die $DBI::errstr;
  print "Updated rows:" + $sth->rows;
  $sth->finish();
  $dbh->commit or die $DBI::errstr;
  ```

## Deleting a record

  ```perl
  my $sth = $dbh->prepare("DELETE FROM DATA_TABLE
                          WHERE ID = 123");
  $sth->execute( $id ) or die $DBI::errstr;
  print "Deleted rows:" + $sth->rows;
  $sth->finish();
  $dbh->commit or die $DBI::errstr;
  ```

