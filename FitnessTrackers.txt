import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';

const FitnessApp = () => {
  const [data, setData] = useState([]);  
  const [input, setInput] = useState({ steps: '', workoutTime: '', calories: '' });

  const calculateCalories = (steps, workoutTime) => {
    const stepCalories = steps * 0.04;     
    const workoutCalories = workoutTime * 8;     
    return stepCalories + workoutCalories;
  };

  const handleAddData = () => {
    const steps = parseInt(input.steps, 10);
    const workoutTime = parseInt(input.workoutTime, 10);
    const calories = calculateCalories(steps, workoutTime);

    setData((prevData) => [
      ...prevData,
      { date: new Date().toLocaleDateString(), steps, workoutTime, calories },
    ]);

    setInput({ steps: '', workoutTime: '', calories: '' });
  };

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setInput((prevInput) => ({ ...prevInput, [name]: value }));
  };

  return (
    <div className="min-h-screen bg-gray-100 flex flex-col items-center p-4">
      <h1 className="text-2xl font-bold mb-4">Fitness Tracker</h1>
      <Card className="mb-4 w-full max-w-md">
        <CardContent>
          <div className="flex flex-col gap-2">
            <label className="font-medium">Steps:</label>
            <input
              type="number"
              name="steps"
              value={input.steps}
              onChange={handleInputChange}
              className="p-2 border rounded"
            />
            <label className="font-medium">Workout Time (minutes):</label>
            <input
              type="number"
              name="workoutTime"
              value={input.workoutTime}
              onChange={handleInputChange}
              className="p-2 border rounded"
            />
            <Button onClick={handleAddData} className="bg-blue-500 text-white">Log Activity</Button>
          </div>
        </CardContent>
      </Card>

      <Card className="w-full max-w-3xl">
        <CardContent>
          <h2 className="text-xl font-semibold mb-2">Progress Summary</h2>
          <LineChart
            width={600}
            height={300}
            data={data}
            margin={{ top: 5, right: 20, left: 0, bottom: 5 }}
          >
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="date" />
            <YAxis />
            <Tooltip />
            <Legend />
            <Line type="monotone" dataKey="calories" stroke="#8884d8" activeDot={{ r: 8 }} />
            <Line type="monotone" dataKey="steps" stroke="#82ca9d" />
          </LineChart>
        </CardContent>
      </Card>
    </div>
  );
};

export default FitnessApp;
