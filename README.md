# qiuqiupackage ianywhere.ultralitej.demo;
import ianywhere.ultralitej.*;
/**
 * CreateDb: sample program to demonstrate Database creation.
 */
public class CreateDb
{
    /**
     * mainline for program.
     *
     * @param args command-line arguments
     *
     */
    public static void main
        ( String[] args )
    {
        try {
            Configuration config = DatabaseManager.createConfigurationFile( "Demo1.ulj" );

            Connection conn = DatabaseManager.createDatabase( config );
            conn.schemaCreateBegin();
    
            TableSchema table_schema = conn.createTable( "Employee" );
            table_schema.createColumn( "number", Domain.INTEGER );
            table_schema.createColumn( "last_name", Domain.VARCHAR, 32 );
            table_schema.createColumn( "first_name", Domain.VARCHAR, 32 );
            table_schema.createColumn( "age", Domain.INTEGER );
            table_schema.createColumn( "dept_no", Domain.INTEGER );
            IndexSchema index_schema = table_schema.createPrimaryIndex( "prime_keys" );
            index_schema.addColumn( "number", IndexSchema.ASCENDING );
    
            table_schema = conn.createTable( "Department" );
            table_schema.createColumn( "dept_no", Domain.INTEGER );
            table_schema.createColumn( "name", Domain.VARCHAR, 50 );
            index_schema = table_schema.createPrimaryIndex( "prime_keys" );
            index_schema.addColumn( "dept_no", IndexSchema.ASCENDING );

            ForeignKeySchema foreign_key_schema = conn.createForeignKey( "Employee", "Department", "fk_emp_to_dept" );
            foreign_key_schema.addColumnReference( "dept_no", "dept_no" );
    
            conn.schemaCreateComplete();

            conn.release();

            Demo.display( "CreateDb completed successfully" );

        } catch( ULjException exc ) {
