Problem statement 1:
Find the number of ratings given by each user.
Problem statement 2:
Find the number of ratings received byeach movie.



//map for problem 1



import java.io.IOException;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.*; 

public class map53 extends Mapper<LongWritable, Text, Text, IntWritable> {
	IntWritable no = new IntWritable(1);
	public void map(LongWritable key, Text value, Context context) 
			throws IOException, InterruptedException {
		String[] lineArray = value.toString().split("\\s+");
		String s = lineArray[0];//stroing the splitted values in strings
		
				
		context.write(new Text(s), no);
		
	}
}


//map for problem 2


import java.io.IOException;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.*; 

public class map53b extends Mapper<LongWritable, Text, Text, IntWritable> {
	IntWritable no = new IntWritable(1);
	public void map(LongWritable key, Text value, Context context) 
			throws IOException, InterruptedException {
		String[] lineArray = value.toString().split("\\s+");
		if(lineArray.length>1)
		{
		String s = lineArray[1];//stroing the splitted values in strings
		
				
		context.write(new Text(s), no);
	}
	}
}


//reducer



import java.io.IOException;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.mapreduce.Reducer;
public class reduce53 extends Reducer<Text,IntWritable,Text, IntWritable> {
	public void reduce(Text key, Iterable<IntWritable> value , Context con) throws IOException, InterruptedException
	{
		int sum =0;
		for(IntWritable v : value)
		{
			sum = sum + v.get();
		}
		con.write(key, new IntWritable(sum));
	}

}




//driver





import org.apache.hadoop.fs.Path; 
import org.apache.hadoop.conf.*;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat; 
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat; 
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat; 
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;

public class driver53 {
	@SuppressWarnings("deprecation")
	public static void main(String[] args) throws Exception {
		Configuration conf = new Configuration();
		Job job = new Job(conf, "DemoTask1");
		job.setJarByClass(driver53.class);

		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(IntWritable.class);

		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		job.setMapperClass(map53a.class);
		job.setReducerClass(reduce53.class);
		job.setInputFormatClass(TextInputFormat.class);
		job.setOutputFormatClass(TextOutputFormat.class);

		FileInputFormat.addInputPath(job, new Path(args[0])); 
		FileOutputFormat.setOutputPath(job,new Path(args[1]));
		
		/*
		Path out=new Path(args[1]);
		out.getFileSystem(conf).delete(out);
		*/
		
		job.waitForCompletion(true);// wait for the job to end
		
		Job job1 = new Job(conf, "DemoTask2");
		job1.setJarByClass(driver53.class);

		job1.setMapOutputKeyClass(Text.class);
		job1.setMapOutputValueClass(IntWritable.class);

		job1.setOutputKeyClass(Text.class);
		job1.setOutputValueClass(IntWritable.class);
		job1.setMapperClass(map53b.class);
		job1.setReducerClass(reduce53.class);
		job1.setInputFormatClass(TextInputFormat.class);
		job1.setOutputFormatClass(TextOutputFormat.class);

		FileInputFormat.addInputPath(job1, new Path(args[0])); 
		FileOutputFormat.setOutputPath(job1,new Path(args[2]));
		
		/*
		Path out=new Path(args[1]);
		out.getFileSystem(conf).delete(out);
		*/
		
		job1.waitForCompletion(true);// wait for the job to end
	}}




*Attaching the output documents for both the problems.
