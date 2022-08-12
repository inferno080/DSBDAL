# map-reduce

# Part 1 - The Code

## step 1.

Create `New > Java Project`, name it `<project-name>`

## step 2

Right click on `<project-name>/src` and `Create New Class`, name it `<my-class>`, this must be same as the `<project-name>`

## step 3

use following template for code:

```java
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;


public class <class-name> {
	
	public static class <map-class-name> extends Mapper<Object, Text, Text, IntWritable>{
		public void map(Object key, Text values, Context context) throws IOException, InterruptedException{
			String data = values.toString();
			String[] lines = data.split("\n");
			for(String line : lines){
				String[] cells = line.split(",");
        // additional logic as per question
				context.write(new Text(cells[1]), new IntWritable(Integer.parseInt(cells[2])));
			}
			
		}
	}
	
	public static class <reduce-class-name> extends Reducer<Text, IntWritable, Text, IntWritable>{
		public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException{
			int sum = 0;
      
			for(IntWritable val : values){
				sum += val.get();
			}
      // additional logic as per question
			context.write(key, new IntWritable(sum));
		}
	}
	
	public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException{
		Configuration conf = new Configuration();
		Job job = Job.getInstance(conf, "job name");
		
		job.setJarByClass(<class-name>.class);
		job.setMapperClass(<map-class-name>.class);
		job.setCombinerClass(<reduce-class-name>.class);
		job.setReducerClass(<reduce-class-name>.class);
		
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		
		FileInputFormat.addInputPath(job, new Path(args[0]));
		FileOutputFormat.setOutputPath(job, new Path(args[1]));
		
		System.exit(job.waitForCompletion(true) ? 0 : 1);
	}

}

```

## step 4

### 4.1
Right Click on `<project-name>` and select `Build Path > Configure Build Path`

### 4.2
Select `Add External JARs` and add `hadoop-common.jar` (inside `hadoop` folder) along with `hadoop-mapreduce-client-core.jar` (inside `hadoop-mapreduce` folder)

## step 5
Right Click on `<project-name>` and Export as JAR file. Make sure all files of `<project-name>` are selected while exporting

# Part 2 - Hadoop

## step 1
Create `input` folder for `.csv` file with `hadoop fs -mkdir input`

## step 2
Add `.csv` to the above path with `hadoop fs -copyFromLocal <local-path-of-file> input`

NOTE: Do not create any `output` folder, it is automatically created.

## step 3
Execute `JAR` file with `hadoop jar <exported-jar-file-location> <class-name> input output`

# step 4
View ouput of map reduce with `hadoop fs -cat output/part-r-00000`
