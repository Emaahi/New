'use client'

import { useState } from 'react'
import { Calendar, momentLocalizer } from 'react-big-calendar'
import moment from 'moment'
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"
import 'react-big-calendar/lib/css/react-big-calendar.css'

// Set up the localizer for react-big-calendar
const localizer = momentLocalizer(moment)

export default function AppointmentBooking() {
  const [selectedProvider, setSelectedProvider] = useState('')
  const [selectedDate, setSelectedDate] = useState(null)
  const [selectedTime, setSelectedTime] = useState('')
  const [name, setName] = useState('')
  const [email, setEmail] = useState('')
  const [bookingConfirmed, setBookingConfirmed] = useState(false)

  const providers = [
    { id: '1', name: 'Neurodevelopmental Pediatrician (Doctor)' },
    { id: '2', name: 'Psychologist (Doctor)' },
    { id: '3', name: 'Psychiatrist (Doctor)' },
    { id: '4', name: 'Behaviour Therapist (Therapist)' },
    { id: '5', name: 'Speech Therapist (Therapist)' },
    { id: '6', name: 'Occupational Therapist (Therapist)' },
    { id: '7', name: 'Physiotherapist (Therapist)' },
    { id: '8', name: 'Dietician / Nutritionist (Therapist)' },
  ]

  const events = [
    {
      title: 'Available',
      start: new Date(2023, 4, 15, 9, 0),
      end: new Date(2023, 4, 15, 10, 0),
    },
    {
      title: 'Available',
      start: new Date(2023, 4, 15, 11, 0),
      end: new Date(2023, 4, 15, 12, 0),
    },
    // Add more available time slots as needed
  ]

  const handleSelectSlot = (slotInfo) => {
    setSelectedDate(slotInfo.start)
    setSelectedTime(moment(slotInfo.start).format('HH:mm'))
  }

  const handleBookAppointment = (e) => {
    e.preventDefault()
    // Here you would typically send the booking information to your backend
    console.log('Booking appointment:', { selectedProvider, selectedDate, selectedTime, name, email })
    setBookingConfirmed(true)
  }

  return (
    <div className="container mx-auto p-4">
      <Card className="w-full max-w-4xl mx-auto">
        <CardHeader>
          <CardTitle>Book an Appointment</CardTitle>
          <CardDescription>Schedule a visit with a doctor or therapist</CardDescription>
        </CardHeader>
        <CardContent>
          {!bookingConfirmed ? (
            <form onSubmit={handleBookAppointment} className="space-y-4">
              <div className="space-y-2">
                <Label htmlFor="provider">Select Provider</Label>
                <Select onValueChange={setSelectedProvider} required>
                  <SelectTrigger id="provider">
                    <SelectValue placeholder="Choose a provider" />
                  </SelectTrigger>
                  <SelectContent>
                    {providers.map((provider) => (
                      <SelectItem key={provider.id} value={provider.id}>
                        {provider.name}
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>
              <div className="space-y-2">
                <Label>Select Date and Time</Label>
                <Calendar
                  localizer={localizer}
                  events={events}
                  startAccessor="start"
                  endAccessor="end"
                  style={{ height: 400 }}
                  onSelectSlot={handleSelectSlot}
                  selectable
                  defaultView="week"
                />
              </div>
              {selectedDate && (
                <div className="space-y-2">
                  <p>Selected Date: {moment(selectedDate).format('MMMM D, YYYY')}</p>
                  <p>Selected Time: {selectedTime}</p>
                </div>
              )}
              <div className="space-y-2">
                <Label htmlFor="name">Name</Label>
                <Input id="name" value={name} onChange={(e) => setName(e.target.value)} required />
              </div>
              <div className="space-y-2">
                <Label htmlFor="email">Email</Label>
                <Input id="email" type="email" value={email} onChange={(e) => setEmail(e.target.value)} required />
              </div>
              <Button type="submit" className="w-full">Book Appointment</Button>
            </form>
          ) : (
            <div className="text-center">
              <h2 className="text-2xl font-bold mb-2">Appointment Confirmed!</h2>
              <p>Thank you for booking an appointment. We'll send a confirmation email shortly.</p>
            </div>
          )}
        </CardContent>
      </Card>
    </div>
  )
}
